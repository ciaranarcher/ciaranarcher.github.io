---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---
<script>
{% include segment.js %}

analytics.ready(function() {
    // Identify
    analytics.track('Page View', {
      title: pageTitle,
      course: courseName
    });

    console.log("tracked the page")
});
</script>
