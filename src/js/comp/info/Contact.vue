<!--
    Copyright (c) 2017, German Neuroinformatics Node (G-Node)

    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted under the terms of the BSD License. See
    LICENSE file in the root of the Project.
-->

<template>
    <div class="container">
        <h2>Contact</h2>
        <hr/><br>
        <dl class="dl-horizontal">
            <dt>G-Node</dt>
            <dd>Biozentrum</dd>
            <dd>Ludwig-Maximilians-Universität München</dd>
            <dd>Großhaderner Straße 2</dd>
            <dd>82152 Martinsried-Planegg</dd>
            <dd>Germany</dd>
        </dl>
        <br/>
        <dl class="dl-horizontal">
            <dt>Phone</dt>
            <dd>+49 (0)89 2180 74810</dd>
            <dt>Fax</dt>
            <dd>+49 89 2180 74803</dd>
            <dt>eMail</dt>
            <dd><a :href="mailto">{{ contact }}</a></dd>
        </dl>
        <br/>
        <dl class="dl-horizontal">
            <dt></dt>
            <dd><a href="https://goo.gl/maps/WxwiKB9Kyo62">G-Node location</a></dd>
        </dl>
    </div>
</template>

<script>
    export default {
        computed: {
            mailto: function() {
                return "mailto:"+ window.api.config.contact_email
            },
            contact: function() {
                return window.api.config.contact_email
            },
        },
    }
</script>
