<template>
    <div class="menu-wrapper" :class="{ 'layout-sidebar-active' : sidebarActive}"
        @click="onMenuClick" @mouseover="onSidebarMouseOver" @mouseleave="onSidebarMouseLeave">
        <div class="sidebar-logo">
            <router-link to="/">
                <img src="assets/layout/images/logo-freya-single.svg" alt="freya-layout">
            </router-link>
            <a class="sidebar-pin" @click="onToggleMenu($event)">
                <span class="pin"></span>
            </a>
        </div>

        <div class="layout-menu-container">
            <AppSubmenu :items="menu" :layoutMode="layoutMode" :parentMenuItemActive="true" :menuActive="menuActive" :mobileMenuActive="mobileMenuActive" :root="true" @menuitem-click="onMenuItemClick" @root-menuitem-click="onRootMenuItemClick" />
        </div>
    </div>
</template>

<script>
import AppSubmenu from "./AppSubmenu";
export default {
    name: "AppMenu",
    emits: ["menu-click", "menuitem-click", "root-menuitem-click", "toggle-menu", "sidebar-mouse-over", "sidebar-mouse-leave"],
    props: {
        sidebarActive: Boolean,
        sidebarStatic: Boolean,
        layoutMode: String,
        menuActive: Boolean,
        mobileMenuActive: Boolean
    },
    data() {
        return {
            menu: [
                {label: "Dashboard", icon: "pi pi-fw pi-home", to: "/"},
                {
                    label: "UI Kit", icon: "pi pi-fw pi-star-o",
                    items: [
                        {label: "Form Layout", icon: "pi pi-fw pi-id-card", to: "/formlayout"},
                        {label: "Input", icon: "pi pi-fw pi-check-square", to: "/input"},
                        {label: "Float Label", icon: "pi pi-fw pi-bookmark", to: "/floatlabel"},
                        {label: "Invalid State", icon: "pi pi-fw pi-exclamation-circle", to: "/invalidstate"},
                        {label: "Button", icon: "pi pi-fw pi-mobile", to: "/button", class: 'rotated-icon'},
                        {label: "Table", icon: "pi pi-fw pi-table", to: "/table"},
                        {label: "List", icon: "pi pi-fw pi-list", to: "/list"},
                        {label: "Tree", icon: "pi pi-fw pi-share-alt", to: "/tree"},
                        {label: "Panel", icon: "pi pi-fw pi-tablet", to: "/panel"},
                        {label: "Overlay", icon: "pi pi-fw pi-clone", to: "/overlay"},
                        {label: "Media", icon: "pi pi-fw pi-image", to: "/media"},
                        {label: "Menu", icon: "pi pi-fw pi-bars", to: "/menu"},
                        {label: "Message", icon: "pi pi-fw pi-comment", to: "/messages"},
                        {label: "File", icon: "pi pi-fw pi-file", to: "/file"},
                        {label: "Chart", icon: "pi pi-fw pi-chart-bar", to: "/chart"},
                        {label: "Misc", icon: "pi pi-fw pi-circle-off", to: "/misc"},
                    ],
                },
                {
                    label: "Utilities", icon: "pi pi-fw pi-compass",
                    items: [
                        {label: "Display", icon:"pi pi-fw pi-desktop", to:"/display"},
                        {label: "Elevation", icon:"pi pi-fw pi-external-link", to:"/elevation"},
                        {label: "Flexbox", icon:"pi pi-fw pi-directions", to:"/flexbox"},
                        {label: "Icons", icon:"pi pi-fw pi-search", to:"/icons"},
                        {label: "Text", icon:"pi pi-fw pi-pencil", to:"/text"},
                        {label: "Widgets", icon:"pi pi-fw pi-star-o", to:"/widgets"},
                        {label: "Grid System", icon:"pi pi-fw pi-th-large", to:"/grid"},
                        {label: "Spacing", icon:"pi pi-fw pi-arrow-right", to:"/spacing"},
                        {label: "Typography", icon:"pi pi-fw pi-align-center", to:"/typography"},
                    ],
                },
                {
                    label: "Pages", icon: "pi pi-fw pi-briefcase",
                    items: [
                        {label: "Crud", icon: "pi pi-fw pi-pencil", to: "/crud"},
                        {label: "Calendar", icon: "pi pi-fw pi-calendar-plus", to: "/calendar"},
                        {label: 'Timeline', icon: 'pi pi-fw pi-calendar', to: '/timeline'},
                        {label: "Landing", icon: "pi pi-fw pi-user-plus", url: "assets/pages/landing.html", target: "_blank"},
                        {label: "Login", icon: "pi pi-fw pi-sign-in", to: "/login"},
                        {label: "Invoice", icon: "pi pi-fw pi-dollar", to: "/invoice"},
                        {label: "Help", icon: "pi pi-fw pi-question-circle", to: "/help"},
                        {label: "Error", icon: "pi pi-fw pi-times-circle", to: "/error"},
                        {label: "Not Found", icon: "pi pi-fw pi-exclamation-circle", to: "/notfound"},
                        {label: "Access Denied", icon: "pi pi-fw pi-lock", to: "/access"},
                        {label: "Empty", icon: "pi pi-fw pi-circle-off", to: "/empty"}
                    ],
                },
                {
                    label: "Hierarchy", icon: "pi pi-fw pi-align-left",
                    items: [
                        {
                            label: "Submenu 1",icon: "pi pi-fw pi-align-left",
                            items: [
                                {
                                    label: "Submenu 1.1", icon: "pi pi-fw pi-align-left",
                                    items: [
                                        {label: "Submenu 1.1.1", icon: "pi pi-fw pi-align-left"},
                                        {label: "Submenu 1.1.2", icon: "pi pi-fw pi-align-left"},
                                        {label: "Submenu 1.1.3", icon: "pi pi-fw pi-align-left"},
                                    ],
                                },
                                {
                                    label: "Submenu 1.2", icon: "pi pi-fw pi-align-left",
                                    items: [
                                        {label: "Submenu 1.2.1", icon: "pi pi-fw pi-align-left"},
                                    ],
                                },
                            ],
                        },
                        {
                            label: "Submenu 2", icon: "pi pi-fw pi-align-left",
                            items: [
                                {
                                    label: "Submenu 2.1", icon: "pi pi-fw pi-align-left",
                                    items: [
                                        {label: "Submenu 2.1.1", icon: "pi pi-fw pi-align-left"},
                                        {label: "Submenu 2.1.2", icon: "pi pi-fw pi-align-left"},
                                    ],
                                },
                                {
                                    label: "Submenu 2.2", icon: "pi pi-fw pi-align-left",
                                    items: [
                                        {label: "Submenu 2.2.1", icon: "pi pi-fw pi-align-left"},
                                    ],
                                },
                            ],
                        },
                    ],
                },
                {
                    label: "Start", icon: "pi pi-fw pi-download",
                    items: [
                        {label: "Buy Now", icon: "pi pi-fw pi-shopping-cart", command: () => window.open("https://www.primefaces.org/store", "_blank")},
                        {label: "Documentation", icon: "pi pi-fw pi-info-circle", to: "/documentation"},
                    ],
                },
            ]
        }
    },
    methods: {
        onSidebarMouseOver() {
            if (this.layoutMode === "sidebar" && !this.sidebarStatic) {
                this.$emit("sidebar-mouse-over");
            }
        },
        onSidebarMouseLeave() {
            if (this.layoutMode === "sidebar" && !this.sidebarStatic) {
                setTimeout(() => {
                    this.$emit("sidebar-mouse-leave");
                }, 250);
            }
        },
        onToggleMenu(event) {
            this.$emit("toggle-menu", event);
        },
        onMenuClick(event) {
            this.$emit("menu-click", event);
        },
        onMenuItemClick(event) {
            this.$emit("menuitem-click", event);
        },
		onRootMenuItemClick(event) {
			this.$emit("root-menuitem-click", event);
		}
    },
    components: { AppSubmenu },
};
</script>