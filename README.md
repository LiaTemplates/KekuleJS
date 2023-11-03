<!--
author:   Yannik Höll

email:    labruzzler@gmail.com

version:  0.0.2

language: en

narrator: US English Female

script: http://cdnjs.cloudflare.com/ajax/libs/raphael/2.1.0/raphael-min.js
        https://cdn.jsdelivr.net/gh/LiaTemplates/KekuleJS/kekule/three.js
        https://cdn.jsdelivr.net/gh/LiaTemplates/KekuleJS/kekule/kekule.min.js

link:   https://cdn.jsdelivr.net/gh/LiaTemplates/KekuleJS/kekule/themes/default/kekule.css



@Kekule.molecule2d: @Kekule._load2d(@uid,cml,@0)

@Kekule.load2d: @Kekule._load2d(@uid,@0,@1)

@Kekule.eval2d: @Kekule._load2d(@uid,@0,`@input`)

@Kekule._load2d
<script>
let rem = document.getElementsByClassName("deleteme")

for( let i=0; i< rem.length; i++ )
  rem[i].innerHTML = ""

const div = document.getElementById("molecule_parent1_@0");

const rawData = `@2`;
const mol = Kekule.IO.loadFormatData(rawData, '@1');
var viewer = new Kekule.ChemWidget.Viewer2D(document, mol);

viewer.removeClassName(Kekule.Widget.HtmlClassNames.NORMAL_BACKGROUND);

div.appendChild(viewer.getElement());

"LIA: stop"
</script>

<div style="text-align: center" id="molecule_parent1_@0" class="deleteme"></div>
@end


@Kekule.molecule3d: @Kekule._load3d(@uid,cml,@0)

@Kekule.eval3d: @Kekule._load3d(@uid,@0,`@input`)

@Kekule.load3d: @Kekule._load3d(@uid,@0,@1)

@Kekule._load3d
<script>
let rem = document.getElementsByClassName("deleteme")

for( let i=0; i< rem.length; i++ )
  rem[i].innerHTML = "";

const div = document.getElementById("molecule_parent2_@0");

const rawData = `@2`;
let mol = Kekule.IO.loadFormatData(rawData, '@1');

var viewer = new Kekule.ChemWidget.Viewer(div);

viewer.setChemObj(mol);
viewer.setZoom(1.5);

viewer.setRenderType(Kekule.Render.RendererType.R3D);
viewer.setAutofit(true);
viewer.setEnableToolbar(true);

"LIA: stop"
</script>

<div data-widget="Kekule.ChemWidget.Viewer" style="width: 100%; height: 550px;" id="molecule_parent2_@0" class="deleteme"></div>
@end

@Kekule.periodicTable: @_periodicTable_(@0, @uid)

@_periodicTable_
<div style="text-align: center" id="periodic_table_@1" class="deleteme"></div>

<script>

let rem = document.getElementsByClassName("deleteme")

for( let i=0; i< rem.length; i++ )
  rem[i].innerHTML = "";

const div = document.getElementById("periodic_table_@1");

const periodic_table = new Kekule.ChemWidget.PeriodicTable(document);

if("@0" !== "large")
  periodic_table.useMiniMode = true;
else
  periodic_table.useMiniMode = false;

periodic_table.setEnableSelect(true)
  .setDisplayedComponents(['symbol', 'name', 'atomicNumber'])
  .appendToElem(div);

console.log("plotting: ", `@0`)
</script>
@end


@Kekule.load: @Kekule.load_(@uid,@0)

@Kekule.load_
<div id="structureContainer_@0"></div>

<script>
// Load the XYZ file from a URL
Kekule.IO.loadUrlData('@1', (mol, success) => {
if (!success) {
  document.getElementById("structureContainer_@0").innerHTML = 'Failed to load @1';
  return;
}

var viewer = new Kekule.ChemWidget.Viewer(document);
viewer.setDimension('100%', '60vh');
viewer.appendToElem(document.getElementById('structureContainer_@0')).setChemObj(mol);  
viewer.setAutofit(true);
viewer.setEnableToolbar(true);
viewer.setRenderType(Kekule.Render.RendererType.R3D);
});

console.log("loading file", Kekule)

</script>

@end

-->

# Kekelu JS

Implementation of the the JS library KekuleJS which provides tool for rendering
molecules and much more chemical notation.

[KekuleJS Source](https://github.com/partridgejiang/Kekule.js/tree/master) <br>
[LiaScript Page](https://liascript.github.io/course/?https://raw.githubusercontent.com/liaTemplates/KekuleJS/master/README.md#1)

# `@Kekule.molecule2d`

Put the cml representation of the molecule you want to render into markdown
quotes and put the macro `@Kekule.molecule2d` on the first line of the markdown
block.

<p style="color: red">You have to be carful that your cml code does not contain any
`@`-Symbols or the macro will not work. (Sometimes these symbols are in the meta info.)</p>

``` xml
<cml xmlns="http://www.xml-cml.org/schema">
  <molecule>
    <atomArray>
      <atom id="a1588768090561" elementType="C" x2="0.4125" y2="0.6348"/>
      <atom id="a1588768090562" elementType="C" x2="-0.4125" y2="0.6348"/>
      <atom id="a1588768090563" elementType="C" x2="-0.6674" y2="-0.1498"/>
      <atom id="a1588768090564" elementType="N" x2="0" y2="-0.6348"/>
      <atom id="a1588768090565" elementType="C" x2="0.6674" y2="-0.1498"/>
    </atomArray>
    <bondArray>
      <bond id="b1588768090566" order="S" atomRefs2="a1588768090561 a1588768090562"/>
      <bond id="b1588768090567" order="D" atomRefs2="a1588768090562 a1588768090563"/>
      <bond id="b1588768090568" order="S" atomRefs2="a1588768090563 a1588768090564"/>
      <bond id="b1588768090569" order="S" atomRefs2="a1588768090564 a1588768090565"/>
      <bond id="b1588768090570" order="D" atomRefs2="a1588768090565 a1588768090561"/>
    </bondArray>
  </molecule>
</cml>
```

The code above gets interpreted as this molecule:

``` xml @Kekule.molecule2d
<cml xmlns="http://www.xml-cml.org/schema">
  <molecule>
    <atomArray>
      <atom id="a1588768090561" elementType="C" x2="0.4125" y2="0.6348"/>
      <atom id="a1588768090562" elementType="C" x2="-0.4125" y2="0.6348"/>
      <atom id="a1588768090563" elementType="C" x2="-0.6674" y2="-0.1498"/>
      <atom id="a1588768090564" elementType="N" x2="0" y2="-0.6348"/>
      <atom id="a1588768090565" elementType="C" x2="0.6674" y2="-0.1498"/>
    </atomArray>
    <bondArray>
      <bond id="b1588768090566" order="S" atomRefs2="a1588768090561 a1588768090562"/>
      <bond id="b1588768090567" order="D" atomRefs2="a1588768090562 a1588768090563"/>
      <bond id="b1588768090568" order="S" atomRefs2="a1588768090563 a1588768090564"/>
      <bond id="b1588768090569" order="S" atomRefs2="a1588768090564 a1588768090565"/>
      <bond id="b1588768090570" order="D" atomRefs2="a1588768090565 a1588768090561"/>
    </bondArray>
  </molecule>
</cml>
```

# `@Kekule.molecule3d`

Put the cml representation of the molecule you want to render into markdown
quotes and put the macro `@Kekule.molecule3d` on the first line of the markdown
block.

<p style="color: red">You have to be carful that your cml code does not contain any
`@`-Symbols or the macro will not work. (Sometimes these symbols are in the meta info.)</p>

``` xml
<cml xmlns="http://www.xml-cml.org/schema">
  <molecule id="m1">
    <atomArray>
      <atom id="a4" elementType="C" x2="13.1708" y2="37.8126"/>
      <atom id="a11" elementType="N" x2="12.3867" y2="37.557500000000005"/>
      <atom id="a5" elementType="C" x2="13.1708" y2="38.637600000000006"/>
      <atom id="a3" elementType="N" x2="13.8865" y2="37.3986"/>
      <atom id="a14" elementType="C" x2="12.3867" y2="35.91630000000001"/>
      <atom id="a7" elementType="C" x2="11.9012" y2="38.225"/>
      <atom id="a6" elementType="C" x2="13.8865" y2="39.050000000000004"/>
      <atom id="a8" elementType="N" x2="12.3867" y2="38.8926"/>
      <atom id="a2" elementType="C" x2="14.6007" y2="37.8126"/>
      <atom id="a15" elementType="O" x2="11.270199999999999" y2="36.330200000000005"/>
      <atom id="a13" elementType="C" x2="11.9655" y2="35.432300000000005"/>
      <atom id="a21" elementType="H" x2="12.3867" y2="35.2079"/>
      <atom id="a1" elementType="N" x2="14.6007" y2="38.637600000000006"/>
      <atom id="a9" elementType="O" x2="13.8865" y2="39.875"/>
      <atom id="a10" elementType="N" x2="15.3178" y2="37.3986"/>
      <atom id="a16" elementType="C" x2="10.1478" y2="35.91630000000001"/>
      <atom id="a12" elementType="C" x2="10.5778" y2="35.432300000000005"/>
      <atom id="a19" elementType="H" x2="11.9655" y2="35.858000000000004"/>
      <atom id="a17" elementType="H" x2="11.9655" y2="34.88720000000001"/>
      <atom id="a22" elementType="C" x2="10.1478" y2="36.578"/>
      <atom id="a23" elementType="H" x2="10.1478" y2="35.2079"/>
      <atom id="a18" elementType="O" x2="10.5778" y2="34.88720000000001"/>
      <atom id="a20" elementType="H" x2="10.5778" y2="35.858000000000004"/>
      <atom id="a24" elementType="O" x2="9.4322" y2="36.9919"/>
    </atomArray>
    <bondArray>
      <bond id="b25" order="S" atomRefs2="a4 a11"/>
      <bond id="b4" order="D" atomRefs2="a4 a5"/>
      <bond id="b3" order="S" atomRefs2="a4 a3"/>
      <bond id="b23" order="S" atomRefs2="a11 a14"/>
      <bond id="b26" order="S" atomRefs2="a11 a7"/>
      <bond id="b5" order="S" atomRefs2="a5 a6"/>
      <bond id="b8" order="S" atomRefs2="a5 a8"/>
      <bond id="b2" order="D" atomRefs2="a3 a2"/>
      <bond id="b13" order="S" atomRefs2="a14 a15"/>
      <bond id="b12" order="S" atomRefs2="a14 a13"/>
      <bond id="b20" order="S" atomRefs2="a14 a21"/>
      <bond id="b7" order="D" atomRefs2="a7 a8"/>
      <bond id="b6" order="S" atomRefs2="a6 a1"/>
      <bond id="b9" order="D" atomRefs2="a6 a9"/>
      <bond id="b1" order="S" atomRefs2="a2 a1"/>
      <bond id="b10" order="S" atomRefs2="a2 a10"/>
      <bond id="b14" order="S" atomRefs2="a15 a16"/>
      <bond id="b11" order="S" atomRefs2="a13 a12"/>
      <bond id="b18" order="S" atomRefs2="a13 a19"/>
      <bond id="b16" order="S" atomRefs2="a13 a17"/>
      <bond id="b15" order="S" atomRefs2="a16 a12"/>
      <bond id="b21" order="S" atomRefs2="a16 a22"/>
      <bond id="b22" order="S" atomRefs2="a16 a23"/>
      <bond id="b17" order="S" atomRefs2="a12 a18"/>
      <bond id="b19" order="S" atomRefs2="a12 a20"/>
      <bond id="b24" order="S" atomRefs2="a22 a24"/>
    </bondArray>
  </molecule>
</cml>
```

The code above gets interpreted as this molecule:

``` @Kekule.molecule3d
<cml xmlns="http://www.xml-cml.org/schema">
  <molecule id="m1">
    <atomArray>
      <atom id="a4" elementType="C" x2="13.1708" y2="37.8126"/>
      <atom id="a11" elementType="N" x2="12.3867" y2="37.557500000000005"/>
      <atom id="a5" elementType="C" x2="13.1708" y2="38.637600000000006"/>
      <atom id="a3" elementType="N" x2="13.8865" y2="37.3986"/>
      <atom id="a14" elementType="C" x2="12.3867" y2="35.91630000000001"/>
      <atom id="a7" elementType="C" x2="11.9012" y2="38.225"/>
      <atom id="a6" elementType="C" x2="13.8865" y2="39.050000000000004"/>
      <atom id="a8" elementType="N" x2="12.3867" y2="38.8926"/>
      <atom id="a2" elementType="C" x2="14.6007" y2="37.8126"/>
      <atom id="a15" elementType="O" x2="11.270199999999999" y2="36.330200000000005"/>
      <atom id="a13" elementType="C" x2="11.9655" y2="35.432300000000005"/>
      <atom id="a21" elementType="H" x2="12.3867" y2="35.2079"/>
      <atom id="a1" elementType="N" x2="14.6007" y2="38.637600000000006"/>
      <atom id="a9" elementType="O" x2="13.8865" y2="39.875"/>
      <atom id="a10" elementType="N" x2="15.3178" y2="37.3986"/>
      <atom id="a16" elementType="C" x2="10.1478" y2="35.91630000000001"/>
      <atom id="a12" elementType="C" x2="10.5778" y2="35.432300000000005"/>
      <atom id="a19" elementType="H" x2="11.9655" y2="35.858000000000004"/>
      <atom id="a17" elementType="H" x2="11.9655" y2="34.88720000000001"/>
      <atom id="a22" elementType="C" x2="10.1478" y2="36.578"/>
      <atom id="a23" elementType="H" x2="10.1478" y2="35.2079"/>
      <atom id="a18" elementType="O" x2="10.5778" y2="34.88720000000001"/>
      <atom id="a20" elementType="H" x2="10.5778" y2="35.858000000000004"/>
      <atom id="a24" elementType="O" x2="9.4322" y2="36.9919"/>
    </atomArray>
    <bondArray>
      <bond id="b25" order="S" atomRefs2="a4 a11"/>
      <bond id="b4" order="D" atomRefs2="a4 a5"/>
      <bond id="b3" order="S" atomRefs2="a4 a3"/>
      <bond id="b23" order="S" atomRefs2="a11 a14"/>
      <bond id="b26" order="S" atomRefs2="a11 a7"/>
      <bond id="b5" order="S" atomRefs2="a5 a6"/>
      <bond id="b8" order="S" atomRefs2="a5 a8"/>
      <bond id="b2" order="D" atomRefs2="a3 a2"/>
      <bond id="b13" order="S" atomRefs2="a14 a15"/>
      <bond id="b12" order="S" atomRefs2="a14 a13"/>
      <bond id="b20" order="S" atomRefs2="a14 a21"/>
      <bond id="b7" order="D" atomRefs2="a7 a8"/>
      <bond id="b6" order="S" atomRefs2="a6 a1"/>
      <bond id="b9" order="D" atomRefs2="a6 a9"/>
      <bond id="b1" order="S" atomRefs2="a2 a1"/>
      <bond id="b10" order="S" atomRefs2="a2 a10"/>
      <bond id="b14" order="S" atomRefs2="a15 a16"/>
      <bond id="b11" order="S" atomRefs2="a13 a12"/>
      <bond id="b18" order="S" atomRefs2="a13 a19"/>
      <bond id="b16" order="S" atomRefs2="a13 a17"/>
      <bond id="b15" order="S" atomRefs2="a16 a12"/>
      <bond id="b21" order="S" atomRefs2="a16 a22"/>
      <bond id="b22" order="S" atomRefs2="a16 a23"/>
      <bond id="b17" order="S" atomRefs2="a12 a18"/>
      <bond id="b19" order="S" atomRefs2="a12 a20"/>
      <bond id="b24" order="S" atomRefs2="a22 a24"/>
    </bondArray>
  </molecule>
</cml>
```

# `@Kekule.periodicTable`
This macro adds a periodic table to the current liaScript page.

The first argument of the macro can be used for making the table large.
Simply type large into the brackets after the macro.
It is not recommended to put more than one table on the same page.

## `@Kekule.periodicTable - small`

@Kekule.periodicTable

## `@Kekule.periodicTable(large) - large`

@Kekule.periodicTable(large)

## Loading Files with `@Kekule.load`

LiaScript has a special syntax for urls, if you want to pass a URL that should be also a link in ordinary Markdown, simply add an `@` in front of the link:

`@[Kekule.load](/data/example.mol)`

---

@[Kekule.load](data/example.mol)


## Loading Files with `@Kekule.load3d`

``` text @Kekule.load3d(mol)
Picture 1                                                                       
  PPPPPPPP          3D                              

  6 12  0  0  0  0  0  0  0  0  0     
    0.0000    1.8079    1.8079 Cu  0  0  0  1
    3.6157    1.8079    1.8079 Cu  0  0  0  1
    1.8079    0.0000    1.8079 Cu  0  0  0  1
    1.8079    3.6157    1.8079 Cu  0  0  0  1
    1.8079    1.8079    0.0000 Cu  0  0  0  1
    1.8079    1.8079    3.6157 Cu  0  0  0  1
  1  3  1  0  0  0  0
  1  4  1  0  0  0  0
  1  5  1  0  0  0  0
  1  6  1  0  0  0  0
  2  3  1  0  0  0  0
  2  4  1  0  0  0  0
  2  5  1  0  0  0  0
  2  6  1  0  0  0  0
  3  5  1  0  0  0  0
  3  6  1  0  0  0  0
  4  5  1  0  0  0  0
  4  6  1  0  0  0  0
M  END
```

``` text
Picture 1                                                                       
  PPPPPPPP          3D                              

  6 12  0  0  0  0  0  0  0  0  0     
    0.0000    1.8079    1.8079 Cu  0  0  0  1
    3.6157    1.8079    1.8079 Cu  0  0  0  1
    1.8079    0.0000    1.8079 Cu  0  0  0  1
    1.8079    3.6157    1.8079 Cu  0  0  0  1
    1.8079    1.8079    0.0000 Cu  0  0  0  1
    1.8079    1.8079    3.6157 Cu  0  0  0  1
  1  3  1  0  0  0  0
  1  4  1  0  0  0  0
  1  5  1  0  0  0  0
  1  6  1  0  0  0  0
  2  3  1  0  0  0  0
  2  4  1  0  0  0  0
  2  5  1  0  0  0  0
  2  6  1  0  0  0  0
  3  5  1  0  0  0  0
  3  6  1  0  0  0  0
  4  5  1  0  0  0  0
  4  6  1  0  0  0  0
M  END
```
@Kekule.eval3d(mol)