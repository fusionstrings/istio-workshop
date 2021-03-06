---
title: "Exercise 1: Default routing"
weight: 31
menu: true
seriesStart: exercise-1
---

Now that we have our app running on Kubernetes, we can finally get to playing around with Istio. Let's set up some basic traffic routing capabilities for the app.

![Default routing](/istio-workshop/img/default-routing.png)

In Istio, we can set up rules for how to route traffic based on the destination service. This can be achieved with a _RouteRule_ custom resource, which can be written in YAML format similar to other Kubernetes resources. Here's a rule that sends all traffic incoming to our frontend _Service_ to version 1 of the app.

```yaml
apiVersion: config.istio.io/v1alpha2
kind: RouteRule
metadata:
  name: frontend-default
spec:
  destination:
    name: frontend-svc
  route:
  - labels:
      version: v1
```

```shell
workshop $ kubectl create -f apps/frontend/kube/rules/default.yaml
routerule "frontend-default" created
```

And same thing for the backend

```shell
workshop $ kubectl create -f apps/backend/kube/rules/default.yaml
routerule "backend-default" created
```


Now we should get constantly "Version 1" response:

```shell
workshop $ curl $ENDPOINT
<html><title>Version 1</title></html>
<body>
<h1 style="background-color: blue; color: white">Version 1</h1>
<ul><li><a href="/greetings">greetings</a></li></ul>
<form action="/" method="post">
Title:<br/> <input type="text" name="title" /><br />
Text:<br/> <textarea name="text"></textarea><br />
<input type="hidden" name="useEdge" value="false" />
<input type="submit" value="Submit" />
</form>
<a href="?useEdge=true">Enable edge backend</a>
</body>
```

Perfect! We can now access our app through Istio! You can also open the URL in your web browser if you like.

In next step we want to route part of the traffic into the new version, aka. [canary deployment »]({{< ref "exercise-2-canary.md" >}})