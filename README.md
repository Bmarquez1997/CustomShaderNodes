
# ShaderNodesExtra
Utilities for Cycles PyNodes

This Add-on serves the purpose of simplifying the creation of new custom nodes for Cycles, and it can be considered as a better API for custom nodes.
The nodes produced by this addon are GPU compatible, and internally they work as an ordinary node group. The main difference is that it's possible to change completely the node layout and add functionality to the node.

Most of it will work in background, and visibly there's not much to see yet.

Apart from the typical node functions (draw_buttons, draw_buttons_ext, update, free, copy, etc), the nodes integrate the following API(s):

ShaderNodeBase:
  * addNode(nodeType, **nodeAttributes)
  * delNode(node)
  * addInput(socketType, **socketAttributes)
  * delInput(socket)
  * addOutput(socketType, **socketAttributes)
  * delOutput(socket)
  * addLink(socket, socket)
  * delLink(link)

Nodes created with this API should call setupTree() in the init() function, and should have a defaultNodeTree() to define the initial node configuration.

For now, while there isn't any MenuEditor, nodes are in charge defining their menu categories. This is not a good design approach; editing the menu will soon become something aside from the custom nodes.

The addon includes a converter operator, that can automatically write a custom node based in some node group.
To use it, select the node group you want to convert into a script, press SPACE, and look for 'Convert Selected Nodegroup to PyNode'.  Alternatively, a "PyNode" tab has been added with an operator button to trigger the functionality as well. It will prompt for a bl_name and a bl_label. For consistency, bl_name should start with 'ShaderNode', though there's nothing to stop you from naming it whatever they want.
The node will be automatically added to the 'Custom Nodes' category, and to change this, one should add a draw_menu function to the node script (located in the 'Nodes' folder inside the Addon path).

At the moment, I'm also sharing some nodes to show what is possible

* DisplacementBake: is a node to help baking displacement maps in a similar fashion as in ZBrush. (Credits to nudelZ)

* NormalBake: will help the creation of tangent normal maps from a normal vector.

* SwitchFloat: is a simple switch, if the 'Switch' input is set to 0, the output will be the 'value1'; if 1, the output will be 'value2'

* Compare: this node will compare two values and return a boolean. It features '>', '>=', '==', '!=', '<=', '<' and '~='.

* Interpolate: as the name says, it will interpolate between two values. Defaults to Lerp(linear interpolation), but also features 'SmoothStep', 'SmootherStep', 'HighPower' and its inverse, 'Sine' and its inverse, 'Cosine' and 'Catmull-Rom' interpolations.

* Loop: A more complex node that uses a node group as a loop step. The node group requires an input socket named 'iterator' for feeding the node with the step integer, and at least 1 output with the same name as the input for the data that is transfered from each cycle to the next.
