![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/02a5662c-e9a6-4476-9209-8a96f3fde0f2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/02a5662c-e9a6-4476-9209-8a96f3fde0f2/Untitled.png)

# FAQ - What is the rewrite-target option?

Different ingress controllers have different options that can be used to customise the way it works. NGINX Ingress controller has many options that can be seen [here](https://kubernetes.github.io/ingress-nginx/examples/). I would like to explain one such option that we will use in our labs. The [Rewrite](https://kubernetes.github.io/ingress-nginx/examples/rewrite/) target option.

Our `watch` app displays the video streaming webpage at `http://<watch-service>:<port>/`

Our `wear` app displays the apparel webpage at `http://<wear-service>:<port>/`

We must configure Ingress to achieve the below. When user visits the URL on the left, his request should be forwarded internally to the URL on the right. Note that the /watch and /wear URL path are what we configure on the ingress controller so we can forwarded users to the appropriate application in the backend. The applications don't have this URL/Path configured on them:`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/`

`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/`

Without the `rewrite-target` option, this is what would happen:

`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/watch`

`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/wear`

Notice `watch` and `wear` at the end of the target URLs. The target applications are not configured with `/watch` or `/wear` paths. They are different applications built specifically for their purpose, so they don't expect `/watch` or `/wear` in the URLs. And as such the requests would fail and throw a `404` not found error.

To fix that we want to "ReWrite" the URL when the request is passed on to the watch or wear applications. We don't want to pass in the same path that user typed in. So we specify the `rewrite-target` option. This rewrites the URL by replacing whatever is under `rules->http->paths->path` which happens to be `/pay` in this case with the value in `rewrite-target`. This works just like a search and replace function.

For example: `replace(path, rewrite-target)`In our case: `replace("/path","/")`

`1. apiVersion: extensions/v1beta1
2. kind: Ingress
3. metadata:
4.   name: test-ingress
5.   namespace: critical-space
6.   annotations:
7.     nginx.ingress.kubernetes.io/rewrite-target: /
8. spec:
9.   rules:
10.   - http:
11.       paths:
12.       - path: /pay
13.         backend:
14.           serviceName: pay-service
15.           servicePort: 8282`

In another example given [here](https://kubernetes.github.io/ingress-nginx/examples/rewrite/), this could also be:

`replace("/something(/|$)(.*)", "/$2")`

`1. apiVersion: extensions/v1beta1
2. kind: Ingress
3. metadata:
4.   annotations:
5.     nginx.ingress.kubernetes.io/rewrite-target: /$2
6.   name: rewrite
7.   namespace: default
8. spec:
9.   rules:
10.   - host: rewrite.bar.com
11.     http:
12.       paths:
13.       - backend:
14.           serviceName: http-svc
15.           servicePort: 80
16.         path: /something(/|$)(.*)`
