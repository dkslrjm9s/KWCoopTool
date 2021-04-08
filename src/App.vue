<template>
    <div :class="containerClass" :data-theme="colorScheme" @click="onDocumentClick($event)">
        <div class="layout-content-wrapper">
            <AppTopBar :sidebarActive="sidebarActive" :sidebarStatic="sidebarStatic" :layoutMode="layoutMode" :topbarTheme="topbarTheme" @right-menubutton-click="onRightMenuButtonClick" 
                :topbarUserMenuActive="topbarUserMenuActive" :topbarUserMenuClick="topbarUserMenuClick" @topbar-usermenu-click="onTopbarUserMenuClick" @toggle-menu="onToggleMenu"
                :searchActive="searchActive" :searchClick="searchClick" :topbarItemClick="topbarItemClick" :activeTopbarItem="activeTopbarItem"
                @sidebar-mouse-leave="onSidebarMouseLeave" @sidebar-mouse-over="onSidebarMouseOver" @topbar-search-click="onTopbarSearchClick" ></AppTopBar>

            <div class="layout-main">
                <div class="layout-content">
                    <router-view/>
                </div>
                <AppFooter/>
            </div>
        </div>

        <AppRightMenu v-model:rightMenuClick="rightMenuClick" :rightMenuActive="rightMenuActive" @right-menu-click="onRightMenuClick"></AppRightMenu>

        <AppConfig v-model:configActive="configActive" v-model:layoutMode="layoutMode" v-model:menuTheme="menuTheme" v-model:colorScheme="colorScheme" v-model:topbarTheme="topbarTheme" @config-click="onConfigClick" @config-button-click="onConfigButtonClick"
            @change-color-scheme="changeColorScheme" @change-component-theme="changeComponentTheme"></AppConfig>

        <div class="layout-mask modal-in"></div>
    </div>
</template>

<script>
import EventBus from './event-bus';
import AppTopBar from "./AppTopbar";
import AppFooter from "./AppFooter";
import AppConfig from "./AppConfig";
import AppRightMenu from "./AppRightMenu";
export default {
    data() {
        return {
            menuActive: false,
            layoutMode: "sidebar",
            colorScheme: "light",
            topbarTheme: "light",
            menuTheme: "light",
            // overlayMenuActive: false,
            sidebarActive: false,
            sidebarStatic: false,
            staticMenuDesktopInactive: false,
            staticMenuMobileActive: false,
            menuClick: false,
            searchActive: false,
            searchClick: false,
            topbarItemClick: false,
            activeTopbarItem: null,
            userMenuClick: false,
            topbarUserMenuActive: false,
            topbarUserMenuClick: false,
            rightMenuClick: false,
            rightMenuActive: false,
            configActive: false,
            configClick: false
        };
    },
    computed: {
        containerClass() {
            return [
                "layout-wrapper",
                {
                    "layout-sidebar": this.layoutMode === "sidebar",
                    "layout-static": this.layoutMode === "sidebar" && this.sidebarStatic,
                    "layout-horizontal": this.layoutMode === "horizontal",
                    "layout-slim": this.layoutMode === "slim",
                    "layout-rightpanel-active": this.rightMenuActive,
                    // "layout-static-inactive": this.staticMenuDesktopInactive && this.layoutMode === "static",
                    "p-input-filled": this.$appState.inputStyle === "filled",
                    "p-ripple-disabled": !this.$primevue.config.ripple,
                },
                "layout-menu-" + this.menuTheme + ' layout-topbar-' + this.topbarTheme
            ];
        },
    },
    components: {
        AppTopBar,
        AppFooter,
        AppConfig,
        AppRightMenu,
    },
    watch: {
        $route() {
            this.menuActive = false;
            this.$toast.removeAllGroups();
        },
    },
    methods: {
        onDocumentClick() {
            if (!this.searchClick && this.searchActive) {
                this.searchActive = false;
                this.searchClick = false;
            }

            if (!this.userMenuClick) {
                this.topbarUserMenuActive = false;
            }

            if (!this.topbarUserMenuClick) {
                this.topbarUserMenuActive = false;
            }

            if (!this.rightMenuClick) {
                this.rightMenuActive = false;
            }

            if (!this.menuClick) {
                if (this.isSlim()) {
					EventBus.emit('reset-active-index');
					this.menuActive = false;
                }

                if (this.overlayMenuActive || this.staticMenuMobileActive) {
                    this.hideOverlayMenu();
                }

                this.unblockBodyScroll();
            }

            if (this.configActive && !this.configClick) {
                this.configActive = false;
            }

            this.searchClick = false;
            this.configClick = false;
            this.userMenuClick = false;
            this.rightMenuClick = false;
            this.topbarUserMenuClick = false;
            this.menuClick = false;
        },
        onToggleMenu(event) {
            this.menuClick = true;
            this.sidebarStatic = !this.sidebarStatic;

            event.preventDefault();
        },
        onMenuClick() {
            this.menuClick = true;
        },
        onMenuButtonClick(event) {
            this.menuClick = true;
            this.topbarUserMenuActive = false;
            this.topbarNotificationMenuActive = false;
            this.rightMenuActive = false;

            // if (this.isOverlay()) {
            //     this.overlayMenuActive = !this.overlayMenuActive;
            // }

            if (this.isDesktop()) {
                this.staticMenuDesktopInactive = !this.staticMenuDesktopInactive;
            } else {
                this.staticMenuMobileActive = !this.staticMenuMobileActive;
                if (this.staticMenuMobileActive) {
                    this.blockBodyScroll();
                } else {
                    this.unblockBodyScroll();
                }
            }

            event.preventDefault();
        },
        onMenuItemClick(event) {
			if (!event.item.items) {
				EventBus.emit('reset-active-index');
				this.hideOverlayMenu();
			}
			if (!event.item.items && this.isSlim()) {
                this.menuActive = false;
            }
        },
		onRootMenuItemClick() {
            this.menuActive = !this.menuActive;
        },
        onRightMenuClick() {
			this.rightMenuClick = true;
		},
        onRightMenuButtonClick(event) {
			this.rightMenuClick = true;
			this.rightMenuActive = !this.rightMenuActive;
			this.hideOverlayMenu();
			event.preventDefault();
		},
        onConfigClick() {
            this.configClick = true;
        },
        onConfigButtonClick() {
            this.configActive = !this.configActive;
            this.configClick = true;
        },
        hideOverlayMenu() {
            this.overlayMenuActive = false;
            this.staticMenuMobileActive = false;
            this.unblockBodyScroll();
        },
        changeComponentTheme(theme) {
            this.changeStyleSheetUrl("theme-css", theme, 3);
        },
        changeColorScheme(scheme) {
            this.colorScheme = scheme;
            this.topbarTheme = scheme;
            this.menuTheme = scheme;

            this.changeStyleSheetUrl("layout-css", "layout-" + scheme + ".css", 1);
            this.changeStyleSheetUrl("theme-css", "theme-" + scheme + ".css", 1);
            this.changeLogo();
        },
        changeStyleSheetUrl(id, value, from) {
            const element = document.getElementById(id);
            const urlTokens = element.getAttribute("href").split("/");

            if (from === 1) {
                // which function invoked this function
                urlTokens[urlTokens.length - 1] = value;
            } else if (from === 2) {
                // which function invoked this function
                if (value !== null) {
                    urlTokens[urlTokens.length - 2] = value;
                }
            } else if (from === 3) {
                // which function invoked this function
                urlTokens[urlTokens.length - 2] = value;
            }

            const newURL = urlTokens.join("/");

            this.replaceLink(element, newURL);
        },
        changeLogo() {
            const appLogoLink = document.getElementById("app-logo");
            const mobileLogoLink = document.getElementById("logo-mobile");
            const invoiceLogoLink = document.getElementById("invoice-logo");
            const footerLogoLink = document.getElementById("footer-logo");
            const logoUrl = `assets/layout/images/logo-${this.d_colorScheme === 'light' ? 'dark' : 'white'}.svg`;

            if (appLogoLink) {
                appLogoLink.src = `assets/layout/images/logo-${this.d_colorScheme === 'light' ? this.logoColor : 'white'}.svg`;
            }

            if (mobileLogoLink) {
                mobileLogoLink.src = logoUrl;
            }

            if (invoiceLogoLink) {
                invoiceLogoLink.src = logoUrl;
            }

            if (footerLogoLink) {
                footerLogoLink.src = logoUrl;
            }
        },
        replaceLink(linkElement, href) {
            if (this.isIE()) {
                linkElement.setAttribute("href", href);
            } else {
                const id = linkElement.getAttribute("id");
                const cloneLinkElement = linkElement.cloneNode(true);

                cloneLinkElement.setAttribute("href", href);
                cloneLinkElement.setAttribute("id", id + "-clone");

                linkElement.parentNode.insertBefore(cloneLinkElement, linkElement.nextSibling);

                cloneLinkElement.addEventListener("load", () => {
                    linkElement.remove();
                    cloneLinkElement.setAttribute("id", id);
                });
            }
        },
        isIE() {
            return /(MSIE|Trident\/|Edge\/)/i.test(window.navigator.userAgent);
        },
        blockBodyScroll() {
            if (document.body.classList) {
                document.body.classList.add('blocked-scroll');
            } else {
                document.body.className += ' blocked-scroll';
            }
        },
        unblockBodyScroll() {
            if (document.body.classList) {
                document.body.classList.remove('blocked-scroll');
            } else {
                document.body.className = document.body.className.replace(new RegExp('(^|\\b)' +
                    'blocked-scroll'.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
            }
        },
        isSlim() {
            return this.layoutMode === "slim";
        },
        isDesktop() {
            return window.innerWidth > 991;
        },
        isMobile() {
            return window.innerWidth <= 991;
        },
        onSidebarMouseOver() {
            this.sidebarActive = !this.isMobile();
        },
        onSidebarMouseLeave() {
            this.sidebarActive = false;
        },
        // onTopbarItemClick(item) {
        //     this.topbarItemClick = true;

        //     if (this.activeTopbarItem === item) {
        //         this.activeTopbarItem = null;
        //     } else {
        //         this.activeTopbarItem = item;
        //     }
        // },
        onTopbarSearchClick() {
            this.searchActive = !this.searchActive;
            this.searchClick = !this.searchClick;
        },
        onTopbarUserMenuClick() {
            this.topbarUserMenuActive = !this.topbarUserMenuActive;
            this.topbarUserMenuClick = !this.topbarUserMenuClick;
        },
    },
};
</script>

<style lang="scss">
@import "App.scss";
</style>
