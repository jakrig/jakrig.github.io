---
layout: post
title: Personal pipeline documentation
description: Personal pipeline documentation
img: /icons/pipeline/jakrig_icon.png
category: rigging, pipeline
tag: [maya, rigging, pipeline]
github-docs: https://k-rig.co.uk/jakrig/
---
The "jakrig" pipeline was created for personal use to support work within maya. It's based on various utilites, functions and classes
that were written over the years and compiled into one rigging pipeline. This is a constant work in progress so all the systems and
solutions are subject to change. <a href="https://k-rig.co.uk/jakrig/">Link to documentation.</a>

<h3>Usage</h3>

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
    screw_ctrl.addAttr("speed", at="float", defaultValue=defaultSpeed)
    # Next we need to create an empty transform as a child to our control. You can define any nodeType as an argument. 
    # Default is "transform".
    screw_parent = screw_ctrl.createChild("screw_parent")
    # Now create a multDoubleLinear node
    mdl = DepNode("screw_mdl")
    mdl.create("multDoubleLinear")
    # Let's use the attribute handler to connect our attributes.
    screw_ctrl.a["translate{}.".format(axis.upper())] >> mdl.a["input1"]
    screw_ctrl.a["speed"] >> mdl.a["input2"]
    mdl.a["output"] >> screw_parent.a["rotate{}".format(axis.upper())]
    # Finally connect the parent group to the screw_mesh. You can use either a parentConstraint or a matrixConstraint.
    constraints.matrixConstraint(screw_parent, mesh)
```

This is a very basic and simple example but as you can see, everything that's handled by maya (and more) can be done using these meta classes.
This is best used as base for granular, automated rigging solutions. 
Using this provides me with complete freedom and I'm not bound to the limitations of mayas internal systems. 
It also provides me with a much easier way to compile, save and load metaData.

Here is a short example of how it would be used in an auto rigging context.

```python
from jakrig.metaNodes import DepNode, DagNode, Joint
from jakrig.metaNodes.control.controlTypes import Control
from jakrig.utils import api, attribute, curve, selection, size
from jakrig.parenting import constraints
from jakrig.rigging.jakrigCreator.jakrigCore import Core 

class MatrixIKSpline(Core):
    type = 'isSplineIKCurve'
    def __init__(self, curve, joints=5, controls=4):
        DagNode.__init__(self, curve)

        # variables
        self._curve = Control(curve)
        self._shape = self.curve.shapes[0]
        self._jointNumber = joints
        self._controlNumber = controls

    @property
    def rootControl(self):
        control = attribute.getRelativesFirst(
            self.curve,
            'isSplineRootControl',
            destination=True
        )

        if control:
            return Control(control)

    @rootControl.setter
    def rootControl(self, control):
        attribute.connectRelatives(
            self.curve,
            control,
            'isSplineRootControl'
        )
```

Systems in jakrig that are not yet included in this documentation:
 * jakrig auto rigging solutions
 * jakrig tools
 * jakanim animations tools and utils
 * PyQt dialogs and utils

<h4>Authored by</h4>
Koji H., Robert Joosten




