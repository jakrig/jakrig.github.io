---
layout: post
title: Personal pipeline documentation
description: Personal pipeline documentation
img: /icons/pipeline/jakrig_icon.png
date: 2019-01-07 21:02:36
category: rigging, pipeline
tag: [maya, rigging, pipeline]
github-docs: https://k-rig.co.uk/jakrig/
---

The "jakrig" pipeline was created for personal use to support work within maya. It's based on various utilites, functions and classes
that were written over the years and compiled into one rigging pipeline. This is a constant work in progress so all the systems and
solutions are subject to change. 
This pipeline is for personal use only and is not to be shared. 
<a href="https://k-rig.co.uk/jakrig/">Link to documentation.</a>

<h3>Usage</h3>
These are very basic and simple examples that demonstrate various solutions within this pipeline.
Using this provides me with complete freedom and I'm not bound to the limitations of mayas internal systems. 
Many functions are written using mayas APIs so there are noticeable speed benefits compared to cmds and pymel.
It also provides me with a much easier way to compile, manage, save and load meta data.

<h3>Base Classes</h3>
You can create a base class for any node within maya. The DagNode class is the base class with which we want to create or
initialize an existing node. It has many different functions for handling nodes and attributes.
The Control, Mesh and Joint classes inherit and build upon the DagNode class with unique functions specific to that node type.
Please refer to the documentation to see what each class can do. 
This is best used as base for granular and automated rigging solutions.

```python
from jakrig.metaClass import DagNode, Control, Mesh, Joint, Curve

node = DagNode(node) #you can parse any node into this class

ctrl = Control(control) #use this for controls
ctrl.create("sphere", "blue")

mesh = Mesh(mesh) #use this for meshes
mesh.saveWeightsToFile()

joint = Joint(joint) #use this for joints
joint.localOffsetMatrix(ctrl)

curve = Curve(curve) #use this for curves
curve.createPointsOnCurve(num)
```

<h3>Handling Attributes</h3>
Once you initiated a class you can index attributes. 
There are functions for all attributes and the indexed attibute.
Python operators were overwritten for various cases of attribute handling.
```python
from jakrig.metaClass import DagNode, Control

node = DagNode(node)
ctrl = Control(ctrl)

all_attr = node.a.list()
ctrl = node.a["translate"].set(100) # You can do everything you can do with API in here. Getting, setting, caching etc. 
ctrl.a["IKFK"] >> node.a["weight"]
ctrl.a["IKFK"].connect(node.a["weight"])
```

<h3>Rigging Solutions</h3>
Here is a short example of how it would be used to create an auto rigging system that can be rebuilt and reused with different character setups.
With each system comes a MetaNode that caches all data.
This helps with modifying the rig after creation and to easily rebuild an existing rigging system.
All MetaNodes can be linked to combine different setups and all data will be stored to rebuild the whole rig, or each system on it's own.
In this example we will also inherit from a Base class that has all the functions that every rigging system needs.
```python
from jakrig.metaClass import MetaNode
from jakrig.rigging.jakrigCreator import Core 

class MatrixIKSpline(Core, MetaNode):
    def __init__(self, *args, **kwargs):
        super(MatrixIKSpline, self).__init__(*args, **kwargs)
```

There are also existing auto rigging solutions for biped, quadruped, props and other.
```python
from jakrig.rigging.jakrigCreator import Limb

r_back_leg_system = Limb("r_back_leg", side="right", parentSystem=rig_base_system, attachParent=hip_system.systemDriver)
r_back_leg_system.a["ikHandle"].set("springSolver") #this can also be set manually in the MetaNode or as a kwarg in the build function
r_back_leg_system.build(*args, **kwargs)
```
Each system has different build options that act as kwargs for the build function. 
All build options are baked into the MetaNode so you can either set it directly in the node, 
through code(as in the example above) or by parsing it as a kwarg in the build function.

<h3>Writing simple functions</h3>
Let's build a simple screw setup as an example. 
```python
from jakrig.metaClass import DepNode, DagNode, Control
from jakrig.parenting import constraints
# The only thing that needs to be given in this example is a screw_mesh. For simplicity's sake let's assume
# we are working in 0,0,0 in a y-up, z-forward setting.
def create(mesh, axis="Y", defaultSpeed=360):
    screw_ctrl = Control("screw")
    screw_ctrl.create("cirlce", "yellow")
    screw_ctrl.addAttr("speed", at="float", defaultValue=defaultSpeed)
    screw_parent = screw_ctrl.createChild("screw_parent")
    mdl = DepNode("screw_mdl")
    mdl.create("multDoubleLinear")
    screw_ctrl.a["translate{}.".format(axis.upper())].connect(mdl.a["input1"])
    screw_ctrl.a["speed"].connect(mdl.a["input2"])
    mdl.a["output"] >> screw_parent.a["rotate{}".format(axis.upper())]
    constraints.matrixConstraint(screw_parent, mesh, mo=True)
```

<h4>Authored by</h4>
Koji H., Rob Joosten




