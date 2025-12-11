---
layout: default
---

![](/assets/images/bungeewing/Title_1.png)

# Bungee Prop Wing

This project is the creation of a CAD model of a wing section for a model aircraft, based on an STL file. The model aircraft itself was not designed by me, the plans were purchased.

I completed this project in November-December of 2022. The model aircraft this wing is based on is the [Bungee Prop by 3DLabPrint](https://3dlabprint.com/shop/bungee-prop/). It is made to be 3D printed using [LW-PLA](https://colorfabb.us/lw-pla-gray-silver), which foams when heated for a lighter build. It is printed in several parts and glued together: the part this project is about is highlighted in the following image.

<figure>
    <img src="/assets/images/bungeewing/Introduction_1.png" width=740 height=446/>
    <figcaption>
        Exploded view of the Bungee Prop model. Part of interest is highlighted.
    </figcaption>
</figure>

I found the design of the aircraft interesting as it does not rely on material like wood or carbon fiber to give the wings strength: all the skin and internal structural parts are made of 3D-printed material. In addition, the aircraft is printed with a setting known in the Cura Slicer as "Spiralize Outer Contour," which prints a single-layer wall with no retractions. For the wing sections in particular, it means that in the STL files the wing skin and internal wing spars form a single thin, continuous surface.

In the following section I outline my process for reverse-engineering this wing section in SolidWorks.

## Creating the CAD Model

### Importing a Reference

<figure>
    <img src="/assets/images/bungeewing/Dimensioning_the_STL_1.png" width=1580 height=1088/>
    <figcaption>
        The original STL of the wing section, imported into SolidWorks.
    </figcaption>
</figure>

My model is based entirely on the STL file for the Bungee Prop. I open the file using SolidWorks, which creates a part containing the STL mesh as a graphics body. All reference sketches for my model are created in the STL mesh part. I then move my reference sketch from the STL mesh part to my model by saving them as blocks, then loading them into my model using **Insert Block.** Sometimes I only require measurements from the STL mesh, in which case I create a sketch and mark my dimensions of interest with line segments and dimension features.

### Wing Shell

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/WingShellOutline_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellOutline_2.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellOutline_3.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellOutline_4.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right, top to bottom: <b>(a)</b> Leading/trailing edge sketch in Top plane; <b>(b)</b> Leading/trailing edge sketch in Front plane; <b>(c)</b> Airfoil sections viewed from Top plane; <b>(d)</b> 3D view of Airfoil sections.
    </figcaption>
</figure>

I start by creating reference sketches outlining the projections of the leading and trailing edges on the Top and Front planes. I create a reference airfoil sketch by taking the cross-section of the STL mesh near the root of the wing, and making a sketch based on its outline. I then add airfoil sections along the wing by duplicating and scaling the original airfoil sketch. Note the bands of airfoil sections near the middle and the root of the wing. These add thickened trailing edges at these sections only, to allow adding wing connectors later.

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/WingShellBody_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellBody_2.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellBody_3.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/WingShellBody_4.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right, top to bottom: <b>(a)</b> View of wing with 3D curves added for leading and trailing edges; <b>(b)</b> Wing partially lofted; <b>(c)</b> Wing fully lofted; <b>(d)</b> Enclosing wingtip with planar surface.
    </figcaption>
</figure>

I turn the sketches of the leading and trailing edges into 3D curves, using the **Extruded Surface** and **Project Curve** features. Then I create surface lofts from the airfoil sections using the leading and trailing edges as guide curves. The wingtip is closed off using **Planar Surface**, and I then use **Knit Surface** to combine all the surface lofts into one surface.

This completes the shell of the wing.

### Spars

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/SparRef_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/SparRef_2.png" width=1580 height=1088/>
    </div>
    <div class="centered">
        <img src="/assets/images/bungeewing/SparRef_3.png" style="width: 50%" width=1580 height=1088/>
    </div>
    <figcaption>
        Clockwise, from top left: <b>(a)</b> Section view being applied to STL mesh; <b>(b)</b> View of spar in mesh from the Front plane; <b>(c)</b> Outline of spars in mesh from the Top plane.
    </figcaption>
</figure>

Here I create the reference sketches for the spar over the reference STL mesh. I use the **Section View** tool on the reference STL mesh to see the internals of the wing, and make a sketch block of the spar. I also outline the spars' shape on the Top plane. I do not need to **Section View** for this, the spars are visible on the surface of the mesh.

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/SparXY_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/SparXY_2.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/SparXY_3.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/SparXY_4.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right, top to bottom: <b>(a)</b> Extruded surfaces created from spar sketch in Top plane; <b>(b)</b> Close-up of extruded surface on a single spar; <b>(c)</b> Close-up of extruded surface after splitting and trimming; <b>(d)</b> All surfaces after splitting and trimming.
    </figcaption>
</figure>

I use **Extrude Surface** on the spar outline on the Top plane. After creating such surfaces for all four spars, I **Split** the extruded surfaces and the wing at the intersection edges. I use **Delete/Keep Body** to remove the offset extruded surfaces, and to remove the small strip of wing lying between the extruded surfaces. I then apply **Trim Surface** to trim the remaining extruded surfaces so that it lies inside of the wing.

<figure>
    <img src="/assets/images/bungeewing/SparXsecProj_1.png" width=1580 height=1088/>
    <figcaption>
        Wing with spar drawings in Front plane projected onto inside surfaces.
    </figcaption>
</figure>

I use **Project Curve** to project on the spar surfaces created in the last step.

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/Spar_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Spar_2.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right: <b>(a)</b> Close-up of lofted spar surface; <b>(b)</b> Wing with all lofts created for spar.
    </figcaption>
</figure>


I create lofts using the projected spar sketch as a guide curves. Note that the surfaces from the previous step is not shown, but still exists. It is removed later.

### Attachment Areas

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/Inboard_7.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Inboard_8.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right: <b>(a)</b> Dimensions for attachment area from Front plane in reference STL mesh; <b>(b)</b> Dimensions for attachment area from Top plane in reference STL mesh.
    </figcaption>
</figure>

The wing section has an area for attachment to an adjoining section at the root. Instead of making a reference sketch as a block and inserting it into my model, I dimension the reference STL mesh and create several reference planes in my model. The attachment area is particularly complicated and I feel it is easier to transfer the dimensions over in this way.

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/Inboard_2.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Inboard_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Inboard_4.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Inboard_6.png" width=1580 height=1088/>
    </div>
    <figcaption>
        From left to right, top to bottom: <b>(a)</b> 3D view of attachment area sketches; <b>(b)</b> Front plane view of attachment area sketches; <b>(c)</b> Close-up of attachment area loft; <b>(d)</b> Front plane view of attachment area loft.
    </figcaption>
</figure>

Using reference planes, I create cross-section sketches of the attachment area and I loft them together. Normally I can create a loft between two sketches using only a single feature, but a pair of the sketches here have differing numbers of sides, so for that pair I had to use many small loft features to get the desired result.

<figure>
    <div class="halfgrid">
        <img src="/assets/images/bungeewing/Outboard_1.png" width=1580 height=1088/>
        <img src="/assets/images/bungeewing/Outboard_3.png" width=1580 height=1088/>
    </div>
    <figcaption>
        Left to right: <b>(a)</b> Attachment area in middle of wing section; <b>(b)</b> Whole wing section with highlighted attachment area in middle.
    </figcaption>
</figure>

Although it is not part of the original STL mesh, I add an attachment area in the middle of the wing section, similar to the last section. I add it to experiment with shortening the maximum height when 3D printing.

## Conclusion

<figure style="float: right; width: 50%; height: auto; margin-left: 20px;">
    <img src="/assets/images/bungeewing/Final_STL_1.png" width=1584 height=1085
        style="height:auto;"/>
    <figcaption>
        An example STL export of the final CAD model for the wing.
    </figcaption>
</figure>


This completes the CAD model of the wing. I can hide and/or delete bodies before exporting it as an STL for 3D printing. This lets me experiment with changing the height of the wing during print.

Compared to the original STL, my STL model has quite an irregular mesh. In practice I haven't faced any difficulties as a result of this — Cura slices it in spiralize mode perfectly fine — but other methods of modeling the wing could generate a more regular mesh. I could have modeled the wing section as a solid instead, and extracted the surface from the solid right before converting it to an STL. Or perhaps, instead of parametric modeling, I could have created the mesh myself using a tool like Blender.

Otherwise, the CAD model is serviceable and good enough for 3D printing.