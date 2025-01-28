---
description: >-
  Publishes a subgraph on the control plane. Optionally, you can publish and
  create a subgraph in one step.
---

# Publish

## **Usage**

```css
npx wgc subgraph publish [subgraphName] --schema [schemaFilePath]
```

{% hint style="warning" %}
**Publish** is an irreversible action. However, the change will only be visible to the routers once the composition has been successful. Until then, the routers will operate with the most recent valid composition. Please use [subgraph check](check.md) to understand the impact of your change.
{% endhint %}

## **Description**

The `npx wgc subgraph publish` command enables you to publish a specific subgraph to the Cosmo platform. Publishing a subgraph makes it available for consumption by other services or applications, allowing them to send GraphQL queries and retrieve data from your subgraph. The `[subgraphName]` argument specifies the name of the subgraph you want to publish, while the `--schema` option defines the path to the GraphQL schema file that contains your subgraph's schema definition.

{% hint style="danger" %}
A previously created [Event-Driven Graph (EDG) ](../../federation/event-driven-federated-subscriptions/)cannot be changed into a regular subgraph. Similarly, a regular subgraph cannot be changed into an EDG. Attempting to do either of these actions will result in an error.
{% endhint %}

## **Parameters**

* `[subgraphName]`: The name of the subgraph you want to publish. Make sure to use the correct name of the subgraph you previously created.

## **Options**

{% hint style="warning" %}
If the subgraph has already been created previously, the `routing-url`, `label`, `subscription-url`,`subscription-protocol`, and `websocket-subprotocol parameters`will be ignored. Use [subgraph update](update.md) to update these values.\
\
If creating an [Event-Driven Graph](../../federation/event-driven-federated-subscriptions/) with publish, providing any of the four aforementioned parameters will produce an error.
{% endhint %}

* `--routing-url`: Set the URL for the subgraph's data source. This URL defines the endpoint from where the subgraph will fetch data.
  * Example: `--routing-url http://localhost:4001/graphql`

{% hint style="warning" %}
If you are _creating_ a subgraph for the first time with `subgraph publish`, the `routing-url` parameter is **required for non-Event-Driven Graphs** to set the routing URL.\
\
If you are _not_ creating a subgraph for the first time, the `routing-url` parameter will be ignored. To update the routing URL of an existing subgraph, use [subgraph update](update.md).
{% endhint %}

* `--label`: Assign multiple labels to the new subgraph. Labels are used to categorize and organize subgraphs based on specific criteria (e.g., team, department, project).
  * Example: `--label team=A`

{% hint style="warning" %}
If you are _creating_ a subgraph for the first time with `subgraph publish`, the `label` parameter can be used to set labels. Note that a subgraph will only be considered for a federated graph composition if the subgraph's labels match the labels matcher of that federated graph.\
\
If you are _not_ creating a subgraph for the first time, the `label` parameter will be ignored. To update the labels of an existing subgraph, use [subgraph update](update.md).
{% endhint %}

* `-n, --namespace` : The namespace of the subgraph (Default: "default").
* `--schema`: The file path to the GraphQL schema definition for the subgraph you want to publish. This file should contain the complete schema definition in the GraphQL Schema Definition Language (SDL) format.
  * Example: `--schema ../demo/subgraphs/products/products.graphql`
* `--fail-on-admission-webhook-error` : If set, the command will fail if the admission webhook fails
* `--fail-on-composition-error` : If set, the command will fail if the composition of the monograph fails.
* `--subscription-url:` Optionally, use a different URL for subscription requests. If no subscription URL is provided, the router URL is used for subscriptions.

{% hint style="warning" %}
If you are _creating_ a subgraph for the first time with `subgraph publish`, the `subscription-url` parameter can be used to set the subscription URL.\
\
If you are _not_ creating a subgraph for the first time, the `subscription-url` parameter will be ignored. To update the subscription URL of an existing subgraph, use [subgraph update](update.md).
{% endhint %}

* `--subscription-protocol:` Optionally, set a protocol to use for subscriptions. The available options are:
  * `ws` (default): Negotiate an appropriate protocol over websockets. Both `grapqhl-ws` and `subscription-transport-ws` are supported.
  * `sse`: Use Server-Sent Events with a GET request.
  * `sse_post`: Use Server-Sent events with a POST request.

{% hint style="warning" %}
If you are _creating_ a subgraph for the first time with `subgraph publish`, the `subscription-protocol` parameter can be used to set the subscription protocol.\
\
If you are _not_ creating a subgraph for the first time, the `subscription-protocol` parameter will be ignored. To update the subscription protocol of an existing subgraph, use [subgraph update](update.md).
{% endhint %}

* `--websocket-subprotocol:` The subprotocol to use when subscribing to the subgraph. The supported protocols are auto(default), graphql-ws, and graphql-transport-ws. It should be used only if the subscription protocol is ws.

{% hint style="warning" %}
If you are _creating_ a subgraph for the first time with `subgraph publish`, the `websocket-subprotocol` parameter can be used to set the websocket subprotocol.\
\
If you are _not_ creating a subgraph for the first time, the `websocket-subprotocol` parameter will be ignored. To update the websocket subprotocol of an existing subgraph, use [subgraph update](update.md).
{% endhint %}

* `--suppress-warnings`: This flag suppresses any warnings produced by composition.

## **Example**

### Publish a schema to an existing subgraph.

```bash
npx wgc subgraph publish products --schema ../demo/subgraphs/products/products.graphql
```

### **Create a subgraph and publish a schema in one command**

```bash
npx wgc subgraph publish products --schema ../demo/subgraphs/products/products.graphql --routing-url http://localhost:4001/graphql --label=team=A
```

## **Notes**

* The `npx wgc subgraph publish` command interacts with the Cosmo platform's control plane to publish the specified subgraph.
* Double-check that the path provided for the `--schema` option points to the correct location of the GraphQL schema file you intend to publish.
