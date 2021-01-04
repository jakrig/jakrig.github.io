---
layout: post
title: Anim Pivot using matrices
description: Creating an anim pivot using matrices. 
img: /icons/palpation/icon.png
date: 2018-07-08 19:18:36
category: rigging, tools
tag: [maya, rigging, tools]
---
How to create an animatable pivot using matrices

<h3>Method</h3>

You can find a tutorial on how to create animatable pivots with constraints <a href="https://k-rig.co.uk/jakrig/">here.</a>



def animPivot(source, offsetParentMatrix=False):
    source_x = cmds.xform(source, q=1, ws=1, t=1)
    anim_pivot_parent = cmds.group(n="{}_anim_pivot".format(source), em=1)
    attachment = cmds.group(n="attachment", em=1)
    pivot_move = Control("pivot_move")
    pivot_move.create("circle", "yellow")
    pivot_rotate = Control("pivot_rotate")
    pivot_rotate.create("sphere", "yellow")
    cmds.parent(pivot_rotate.offset, pivot_move)
    cmds.parent(pivot_move.offset, attachment, anim_pivot_parent)
    cmds.xform(anim_pivot_parent, t=source_x, ws=1)
    cmds.parent(anim_pivot_parent, source)

    mm01 = cmds.createNode("multMatrix")
    cmds.connectAttr("{}.inverseMatrix".format(pivot_move), "{}.matrixIn[0]".format(mm01))
    cmds.connectAttr("{}.worldMatrix".format(pivot_rotate), "{}.matrixIn[1]".format(mm01))

    mm02 = cmds.createNode("multMatrix")
    cmds.connectAttr("{}.matrixSum".format(mm01), "{}.matrixIn[0]".format(mm02))
    cmds.connectAttr("{}.worldInverseMatrix".format(anim_pivot_parent), "{}.matrixIn[1]".format(mm02))

    if offsetParentMatrix:
        cmds.connectAttr("{}.matrixSum".format(mm02), "{}.offsetParentMatrix".format(attachment))
    else:
        dm = cmds.createNode("decomposeMatrix")
        cmds.connectAttr("{}.matrixSum".format(mm02), "{}.inputMatrix".format(dm))
        for transform in ["Translate", "Rotate"]:
            cmds.connectAttr("{}.output{}".format(dm, transform), "{}.{}".format(attachment, transform.lower()))
    return attachment, pivot_move.offset 
