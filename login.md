---
layout: page
title: Login
permalink: /login/
---

You have just logged in!


<script>

{% include segment.js %}

analytics.ready(function() {

  // Identify
  internalID = (new URL(window.location)).searchParams.get('id') ||
  databaseID;
  anonID = analytics.user().anonymousId();
  console.log("anonID", anonID);

  analytics.identify(internalID, {
    name: firstName,
    email: email,
    nickname: nickName,
    blogger: true,
    age: 47,
    song: 'coco soundtrack'
  });
  console.log("identified");

  analytics.alias(internalID, anonID);
  console.log("aliased");

});

</script>
