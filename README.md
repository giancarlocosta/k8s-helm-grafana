# Grafana Helm Chart

> Cloned and slightly modified version of
[Stable Kubernetes Grafana Helm Chart](https://github.com/kubernetes/charts/tree/master/stable/grafana).
Refer there for more documentation.


## Getting Started

1. If you are going to use persistence _and you are using some custom storage layer such as NFS_,
deploy the Kubernetes PersistentVolumes and PersistentVolumeClaims you define in
`properties/<your-target-cluster>/pre-install/persistent-volumes` for your target
cluster if you haven't already. _If not using a custom storage layer don't worry about this._

2. Deploy secrets in `properties/<your-target-cluster>/pre-install/secrets`.
> NOTE: secret name must match name you use when installing the chart!

3. Specify/update values in `properties/<your-target-cluster>/properties.yaml` for
your target cluster that will be used to overwrite default values of the Chart.

4. If you want to add custom dashboards in addition to the ones already added from
`common-dashboards/` then set the Helm value `useCustomDashboards: true` and add your
dashboard json files to a `custom-dashboards/` folder in the base dir of this Chart.<br/>

    >**TIP**: keep a folder of custom dashboards in `properties/<your-target-cluster>/custom-dashboards`
    for each target cluster so that all you have to do at deploy-time is copy those dashboards over to the
    `custom-dashboards/` folder in the base dir of this Chart._

    >**GOTCHA**: If you set the Helm value `useCustomDashboards: true` but you do NOT add
    dashboard json files to the `custom-dashboards/` folder in the base dir of this Chart,
    or the `custom-dashboards/` folder doesn't exist, Helm will throw an error!<br/>
    _Never git commit the `custom-dashboards/` folder in the base dir of this Chart.
    Instead keep the custom dashboards for each target cluster in their respective cluster folders
    in `properties/` and commit that._

    > **NOTE**: See `saved-dashboards/` for useful dashboards that you can use
    in addition to any you might find online. Save good ones you find here!

5. Deploy (Helm install/update) using the `properties/` values file for your target cluster.

Example deployment to ELK Prod env
```
helm --kube-context prod-elk --namespace ops --name prod-elk install . --values properties/prod-elk/properties.yaml --debug
helm --kube-context prod-elk --namespace ops upgrade prod-elk . --values properties/prod-elk/properties.yaml --debug
```
