<template>
    <div class="layout-footer">
        <div class="p-grid">
            <div class="p-col-12  p-lg-4">
                <div class="p-grid">
                    <div class="p-col-6 ">
                        <span class="footer-menutitle">SITE MAP</span>
                        <ul>
                            <li><a href="/">Dashboard</a></li>
                            <li><a href="/crud">Crud</a></li>
                            <li><a href="/invoice">Invoice</a></li>
                            <li><a href="/help">Help</a></li>
                        </ul>
                    </div>
                    <div class="p-col-6">
                        <span class="footer-menutitle"></span>
                        <ul>
                            <li><a href="/calendar">Calendar</a></li>
                            <li><a href="/widgets">Widgets</a></li>
                            <li><a href="/documentation">Documentation</a></li>
                            <li><a href="https://www.primefaces.org/store">Buy Now</a></li>
                        </ul>
                    </div>
                </div>

            </div>
            <div class="p-col-12 p-md-6 p-lg-3">
                <span class="footer-menutitle">CONTACT US</span>
                <ul>
                    <li>(415) 931-1624 794</li>
                    <li>Mcallister St San Francisco,</li>
                    <li>California(CA), 94102</li>
                </ul>
            </div>
            <div class="p-col-12 p-md-6 p-lg-5">
                <span class="footer-menutitle">NEWSLETTER</span>
                <span class="footer-subtitle">Join our newsletter to get notification about the new features.</span>
                    <div class="newsletter-input">
                        <InputText type="text" placeholder="email address" />
                        <Button type="button" label="Join" class="p-button-secondary"></Button>
                    </div>
            </div>
            <div class="p-col-12">
                <div class="footer-bottom">
                    <h4>freya</h4>
                    <h6>Copyright Ⓒ PrimeTek Informatics</h6>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "AppFooter"
};
</script>