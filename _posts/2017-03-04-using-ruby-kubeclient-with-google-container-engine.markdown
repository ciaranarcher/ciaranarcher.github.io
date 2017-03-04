---
layout: post
title:  "Fixing auth errors using the Ruby kubeclient and Google Container Engine."
date:   2017-03-04 09:00:52 +0000
categories: gcp ruby gke
---

If you are having difficulty sending commands to GKE when using the
Ruby [kubeclient](https://github.com/abonas/kubeclient) and a GCP service account,
then here is a tip or two to help.

First up, create a service account in the
GCP [IAM console](https://cloud.google.com/iam/docs/creating-managing-service-accounts).

Next activate your service account using a long lived token
as [explained here](https://cloud.google.com/sdk/gcloud/reference/auth/activate-service-account).

Usually when using `kubectl` to authenticate the cluster a short-lived token is created and
written to the `kubeconfig` file. However in our case we want to use a client certificate authentication strategy.

The reason this is important is it is currently compatible with the Ruby [kubeclient](https://github.com/abonas/kubeclient), and thus avoids a lot of frustration and tears.

So run:

{% highlight bash %}
gcloud config set container/use_client_certificate True
{% endhighlight %}

Then as usual:

{% highlight bash %}
gcloud container clusters get-credentials ...
{% endhighlight %}

You can get further details by checking out [this Github issue](https://github.com/abonas/kubeclient/issues/210#issuecomment-272911197).






