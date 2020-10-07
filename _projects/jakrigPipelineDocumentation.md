---
layout: post
title: jakrig personal pipeline
description: Personal pipeline documentation
img: /icons/palpation/icon.png
date: 2020-10-07 21:02:36
category: rigging, pipeline
tag: [maya, rigging, pipeline]
---
The "jakrig" pipeline was created for personal use to support work within maya. It's based on various utilites, functions and classes
that were written over the years and compiled into one rigging pipeline. This is a constant work in progress so all the systems and
solutions are subject to change.

<h3>Usage</h3>

Examples:
```python
# Let's build a simple screw setup as an example.
from jakrig.metaNodes import DepNode, DagNode
from jakrig.metaNodes.control.controlTypes import Control
from jakrig.parenting import constraints
# The only thing that needs to be given in this example is a screw_mesh. For simplicity's sake let's assume
# we are working in 0,0,0 in a y-up, z-forward setting.
def create(mesh, axis="Y", defaultSpeed=360):
    # Create screw control
    screw_ctrl = Control("screw")
    screw_ctrl.create("cirlce", "yellow")
    # Add speed attribute. The default value is by how many degrees you want the screw to turn when the control is
    # translated by 1
    screw_ctrl.addAttr("speed", at="float", defaultValue=360)
    # Next we need to create an empty transform as a child to our control. You can define any nodeType as an argument. 
    # Default is "transform".
    screw_parent = screw_ctrl.createChild("screw_parent")
    # Now create a multDoubleLinear node
    mdl = DepNode("screw_mdl")
    mdl.create("multDoubleLinear")
    # Let's use the attribute handler to connect our attributes.
    screw_ctrl.a["translate{}.".format(axis.upper())] >> mdl.a["input1"]
    screw_ctrl.a["speed"] >> mdl.a["input2"]
    screw_ctrl.a["output"] >> screw_parent.a["rotate{}".format(axis.upper())]
    # Finally connect the parent group to the screw_mesh. You can use either a parentConstraint or a matrixConstraint.
    constraints.matrixConstraint(screw_ctrl, mesh)
```

<h4>Authored by</h4>
Koji H., Robert Joosten




