# GraphGL

<p align="center">
  <img src="/graph.gl/gatsby/images/graph.png" height="200" />
</p>

## Usage

```js
import GraphGL, {
  JSONLoader,
  NODE_TYPE,
  D3ForceLayout
} from 'graph.gl';

const App = ({data}) => {
  const graph = JSONLoader({
    json: data,
    nodeParser: node => ({id: node.id}),
    edgeParser: edge => ({
      id: edge.id,
      sourceId: edge.sourceId,
      targetId: edge.targetId,
      directed: true,
    }),
  });
  return (
    <GraphGL
      graph={graph}
      layout={new D3ForceLayout()}
      nodeStyle={[
        {
          type: NODE_TYPE.CIRCLE,
          radius: 10,
          fill: 'blue',
          opacity: 1,
        },
      ]}
      edgeStyle={{
        stroke: 'black',
        strokeWidth: 2,
      }}
      enableDragging
    />
  );
}
```

## graph (Graph, required)
The graph data will need to be processed through JSONLoader and converted into [`Graph`](docs/api-reference/graph) object.  The expected data should be an object includes two arrays: `nodes` and `edges`. Each node require an unique `id`. Each edge should have `id` as edge ID, `sourceId` as the ID of the source node, and `targetId` as the ID of the target node. For example:
```js
const data = {
  nodes: [
    {id: '1'}, {id: '2'}, {id: '3'},
  ],
  edges: [
    {id: 'e1', sourceId: '1', targetId: '2'},
    {id: 'e2', sourceId: '1', targetId: '3'},
    {id: 'e3', sourceId: '2', targetId: '3'},
  ],
};
```

Then, you can convert the data into `Graph` by `JSONLoader`:
```js
import {JSONLoader} from 'graph.gl';
const graph = JSONLoader({json: data});
```

## layout (Layout, required)
Use one of the layouts provided by Graph.gl or create a new custom layout class by following the [instruction](/docs/advanced/custom-layout). Right now Graph.gl provides D3 and Simple layout for basic usage. There are more experimental layouts under `src/experimental-layouts`, please reference to the experimental layout [gallery](docs/experimental).

## initialViewState (optional)

```js
initialViewState={{
  target: [0, 0],
  zoom: 1,
}}
```
 - target ([x: Number, y: Number], optional):  The target origin to the center of the view.
 - zoom (Number, optional): The zoom level of the view.


## nodeStyle (Array, required)

A node is made of a set of layers. nodeStyle is a set of style objects to describe the style for each layer.
For more detail, please see the explanation of nodeStyle at [here](/docs/api-reference/node-style).

## nodeEvents (Object, required)
All events callbacks will be triggered with the following parameters:
```js
info: {
  object:  The object that was picked.
  x: Mouse position x relative to the viewport.
  y: Mouse position y relative to the viewport.
  coordinate:  Mouse position in viewport coordinate system.
}
```

 - onClick: This callback will be called when the mouse clicks on an node. Default: `null`.
 - onMouseEnter: This callback will be called when the mouse enter an node. Default: `null`.
 - onHover: This callback will be called when the mouse hovers over an node. Default: `null`.
 - onMouseLeave: This callback will be called when the mouse leaves an node. Default: `null`.

## edgeStyle  (Object, required)

For more detail, please see the explanation of edgeStyle at [here](/docs/api-reference/edge-style)

## edgeEvents (Object, required)
All events callbacks will be triggered with the following parameters:
```js
info: {
  object:  The object that was picked.
  x: Mouse position x relative to the viewport.
  y: Mouse position y relative to the viewport.
  coordinate:  Mouse position in viewport coordinate system.
}
```

 - onClick: This callback will be called when the mouse clicks on an edge. Default: `null`.
 - onHover: This callback will be called when the mouse hovers over an edge. Default: `null`.


## Source
[src/graphgl.js](https://github.com/uber-common/graph.gl/blob/master/src/graphgl.js)