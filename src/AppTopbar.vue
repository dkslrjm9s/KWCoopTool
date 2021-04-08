<template>
    <div class="layout-topbar">
        <div class="layout-topbar-wrapper">
            <div class="layout-topbar-left">
                <a tabindex="0" class="menu-button" @click="onMenuButtonClick(event)">
                    <i class="pi pi-bars"></i>
                </a>
                <a tabindex="0" id="logo-link" class="layout-topbar-logo" @click="onMenuButtonClick(event)">
                    <img :src="'assets/layout/images/logo-' + (topbarTheme === 'dark' ? 'freya-white' : 'freya') + '.svg'" alt="freya-layout"/>
                </a>
            </div>

            <AppMenu :layoutMode="layoutMode" :sidebarActive="sidebarActive" :sidebarStatic="sidebarStatic" @sidebar-mouse-leave="onSidebarMouseLeave" @sidebar-mouse-over="onSidebarMouseOver"
                @toggle-menu="onToggleMenu" />

            <div class="layout-topbar-right">
                <ul class="layout-topbar-actions">
                    <li ref="search" class="topbar-item search-item" :class="{'active-topmenuitem': searchActive}">
                        <a tabindex="0" @click="onTopbarSearchClick">
                            <i class="topbar-icon pi pi-search"></i>
                        </a>

                        <div class="search-input-wrapper">
                            <span class="p-input-icon-left">
                                <i class="pi pi-search"></i>
                                <InputText type="text" placeholder="Search..." @click="$emit('update:searchClick', true)" />
                            </span>
                        </div>

                        <ul class="fadeInDown">
                            <div class="search-input-wrapper p-fluid" style="width: 100%">
                                <span class="p-input-icon-left">
                                    <i class="pi pi-search"></i>
                                    <InputText type="text" placeholder="Search..." @click="$emit('update:searchClick', true)" />
                                </span>
                            </div>
                        </ul>
                    </li>

                    <li ref="profile" class="topbar-item user-profile" :class="{'active-topmenuitem fadeInDown': topbarUserMenuActive}">
                        <a href="#" @click="onTopbarUserMenuClick">
                            <img src="assets/layout/images/avatar-profilemenu.png" alt="freya-layout"/>
                        </a>
                        <ul class="fadeInDown">
                            <li>
                                <a href="#">
                                    <span>Profile</span>
                                </a>
                            </li>
                            <li>
                                <a href="#">
                                    <span>Settings</span>
                                </a>
                            </li>
                            <li>
                                <a href="#">
                                    <span>Messages</span>
                                </a>
                            </li>
                            <li>
                                <a href="#">
                                    <span>Notifications</span>
                                </a>
                            </li>
                        </ul>
                    </li>
                </ul>

                <a href="#" tabindex="0" class="layout-rightpanel-button" @click="onRightMenuButtonClick($event)">
                    <i class="pi pi-arrow-left"></i>
                </a>
            </div>
        </div>
    </div>
</template>

<script>
import AppMenu from './AppMenu';
export default {
    name: "AppTopbar",
    emits: ["toggle-menu", "right-menubutton-click", "sidebar-mouse-over", "sidebar-mouse-leave", "topbar-search-click", "topbar-usermenu-click", "update:searchClick"],
    props: {
        searchActive: Boolean,
        searchClick: Boolean,
        topbarItemClick: Boolean,
        topbarUserMenuActive: Boolean,
        topbarUserMenuClick: Boolean,
        activeTopbarItem: String,
        sidebarActive: Boolean,
        sidebarStatic: Boolean,
        layoutMode: String,
        topbarTheme: String
    },
    data() {
        return {           
        };
    },
    unmounted() {
        if (this.subscription) {
            this.subscription.unsubscribe();
        }
    },
    methods: {
        onToggleMenu(event) {
            this.$emit("toggle-menu", event);
        },
        onTopbarSearchClick(event) {
            this.$emit('topbar-search-click', event);
        },
        onTopbarUserMenuClick(event) {
            this.$emit("topbar-usermenu-click", event)
        },
        onRightMenuButtonClick(event) {
            this.$emit("right-menubutton-click", event);
        },
        onSidebarMouseOver() {
            this.$emit("sidebar-mouse-over");
        },
        onSidebarMouseLeave() {
            this.$emit("sidebar-mouse-leave");
        }
    }, 
    components: { 
        AppMenu
    }
};
</script>