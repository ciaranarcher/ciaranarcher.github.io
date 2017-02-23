---
layout: post
title:  "Pushing from Travis to Google Container Registry"
date:   2017-02-23 21:17:52 +0000
categories: gcp travis
---

I was looking for a tutorial to help me push stuff to GCR from Travis, so I've pulled what I found worked together here.
Hopefully this will help others (and my future self!).


* Create a service account (e.g. `travis-ci@<your-project>.iam.gserviceaccount.com`). You can leave the role blank.
* Create a key for the service account, choose the JSON option and download it somewhere safe.
* Rename the file to `google-auth.json` or similar.
* We need to shove this JSON object into a environment variable, so strip all lines breaks from it: `tr '\n' ' ' < google-auth.json` and save the result back into the file.
* Go to Cloud Storage and find the backed called `artifacts.<your-project>.appspot.com`.
* Edit the permissions on the bucket to add your service account created above (`travis-ci@<your-project>.iam.gserviceaccount.com`) as new `User` entity with `Writer` access.
* Test locally:

{% highlight bash %}
gcloud auth activate-service-account travis-ci@<your-project>.iam.gserviceaccount.com --key-file=google-auth.json

Activated service account credentials for: [travis-ci@<your-project>.iam.gserviceaccount.com]
{% endhighlight %}

* You can try push a docker image then to confirm that the access permissions work as expected for Container Registry.
* Next onto Travis…
* Create a repository level env var called `GCLOUD_AUTH_JSON_FILE`
* Run `base64 -i google-auth.json` to encode the json and paste the result into the value field for the env var. Save the repository level env var.
* Add another repository level env var called `GCLOUD_ACC_ID` and set it to the value of the service account ID you created above , i.e.  `travis-ci@<your-project>.iam.gserviceaccount.com`.
* Add another repository level en var called `GCLOUD_PROJECT` and it to your cloud project name.
* Next you can try authenticate and use the GCR  from Travis:
{% highlight bash %}
- echo $GCLOUD_AUTH_JSON_FILE | base64 --decode > gcloud-auth.json
  - gcloud auth activate-service-account $GCLOUD_ACC_ID --key-file gcloud-auth.json
  # Push to GCR
  - docker tag default/zodiac-ingest "eu.gcr.io/$GCLOUD_PROJECT/zodiac-ingest:latest"
  - gcloud docker -- push "eu.gcr.io/$GCLOUD_PROJECT/zodiac-ingest:latest"

{% endhighlight %}

