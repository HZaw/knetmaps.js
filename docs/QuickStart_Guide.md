# Installation Guide

**KnetMaps** is a web application that uses cytoscapeJS, jQuery and other javascript libraries to visualize network graphs and allow users to interact with them.

All KnetMaps requires is a JSON dataset (in a nested JSON format) as input and it then visualizes it within an embedded container on your web page.

### Using KnetMaps out-of-the-box in a static web page

Use a JSON network dataset, containing information about the nodes and edges in a network and (optional) visual attributes for them, in your html page by including it in `<head>`:
```
    <head>
        <link href="css/index-style.css" rel="stylesheet" /> <!-- page stylesheet -->
        <link href="css/knet-style.css" rel="stylesheet" /> <!-- Network Viewer stylesheet -->
        <link href="https://cdnjs.cloudflare.com/ajax/libs/qtip2/2.2.0/jquery.qtip.min.css" rel="stylesheet" type="text/css" />
        <!-- jQuery Ajax MaskLoader: CSS for Loader overlay -->
        <link href="css/maskloader.css" rel="stylesheet">

        <meta charset=utf-8 />
        <script src="libs/jquery-1.11.2.min.js"></script>
        <script src="libs/cytoscapejs_2.4.0/cytoscape.min.js"></script>
        <script src="libs/jquery-ui.js"></script>
        <script src="libs/cytoscape-cxtmenu.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/qtip2/2.2.0/jquery.qtip.min.js"></script>
        <script src="libs/cytoscape-qtip.js"></script>
        <script src="libs/jquery.maskloader.js"></script>
        <!-- Layouts -->
        <script src="libs/cose_bilkent/cytoscape-cose-bilkent.js"></script>
        <script src="libs/ngraph_forceLayout/cytoscape-ngraph.forcelayout.js"></script>

        <!-- URL mappings config file used for generating url's in Item Info -->
        <script type="text/javascript" src="config/url_mappings.json"></script>

	    <!-- JSON dataset for static example -->
        <script src="sampleFiles/simpleDataset1.json"></script>
        
  		<!-- KnetMaps code -->
        <script src="javascript/knet-maskLoader.js"></script>
        <script src="javascript/knet-layouts-defaultParams.js"></script>
        <script src="javascript/knet-layouts.js"></script>
        <script src="javascript/knet-menu.js"></script> <!-- KnetMaps menubar -->
        <script src="javascript/knet-counts-legend.js"></script>
        <script src="javascript/knet-container.js"></script>
        <script src="javascript/knet-toggleFullScreen.js"></script>
        <script src="javascript/knet-itemInfo.js"></script>
        <script src="javascript/knet-generator.js"></script>

	    <!-- test page js -->
        <script src="js/launchNetwork.js"></script>

        <title>KnetMaps.js demo</title>
    </head>
```

You can use the following example JSON dataset:
```
var graphJSON= { "nodes": [
        { "data": { "id": "1", "conceptType": "Gene", "flagged": "true", "conceptColor": "lightBlue", "annotation": "", "conceptSize": "26px", "value": "c1", "pid": "c1", "conceptDisplay": "element", "conceptShape": "triangle"} , "group": "nodes"} , 
        { "data": { "id": "2", "conceptType": "Protein", "flagged": "false","conceptColor": "red", "annotation": "",  "conceptSize": "22px", "value": "POTRI.002G046500", "pid": "POTRI.002G046500.1", "conceptDisplay": "element", "conceptShape": "ellipse"} , "group": "nodes"} , 
		{ "data": { "id": "3", "conceptType": "Protein", "flagged": "false", "conceptColor": "red", "annotation": "Version:  55", "conceptSize": "26px", "value": "AT1G03055", "pid": "AT1G03055.1;Q7XA78", "conceptDisplay": "element", "conceptShape": "ellipse"} , "group": "nodes"} , 
		{ "data": { "id": "4", "conceptType": "Publication", "flagged": "false", "conceptColor": "orange", "annotation": "", "conceptSize": "26px", "value": "PMID: 22623516", "pid": "22623516;PMID: 22623516", "conceptDisplay": "element", "conceptShape": "rectangle"} , "group": "nodes"} , 
		{ "data": { "id": "5", "conceptType": "Molecular_Function", "flagged": "false", "conceptColor": "purple", "annotation": "Elemental activities...", "conceptSize": "18px", "value": "molecular function", "pid": "GO: 0003674", "conceptDisplay": "none", "conceptShape": "pentagon"} , "group": "nodes"} , 
		{ "data": { "id": "6", "conceptType": "Cellular_Component", "flagged": "false", "conceptColor": "springGreen", "annotation": "common type", "conceptSize": "18px", "value": "plastid", "pid": "GO: 0009536", "conceptDisplay": "none", "conceptShape": "pentagon"} , "group": "nodes"} 
		],
    "edges": [
        { "data": { "id": "e1", "source": "1", "relationColor": "grey", "relationSize": "3px", "relationDisplay": "element", "target": "2", "label": "encodes"} , "group": "edges"} , 
        { "data": { "id": "e2", "source": "2", "relationColor": "red", "relationSize": "3px", "relationDisplay": "element", "target": "3", "label": "has_similar_sequence"} , "group": "edges"} , 
        { "data": { "id": "e3", "source": "4", "relationColor": "grey", "relationSize": "3px", "relationDisplay": "element", "target": "3", "label": "encodes"} , "group": "edges"} , 
        { "data": { "id": "e5", "source": "4", "relationColor": "orange", "relationSize": "3px", "relationDisplay": "element", "target": "5", "label": "published_in"} , "group": "edges"} , 
        { "data": { "id": "e6", "source": "1", "relationColor": "teal", "relationSize": "3px", "relationDisplay": "element", "target": "6", "label": "participates_in"} , "group": "edges"} , 
        { "data": { "id": "e8", "source": "4", "relationColor": "springGreen", "relationSize": "1px", "relationDisplay": "none", "target": "6", "label": "located_in"} , "group": "edges"} 
		]};
```

Note: In QTLNetMiner, the dataset file has 2 JSON objects within it a `graphJSON` containing the network information and also an additional JSON object: ```allGraphData``` that provides additional metadata about nodes (synonyms, accessions, evidences) and edges (scores for weighted edges, e.g, p-value, BLAST scores, etc). This information is displayed in a sliding overlay panel called **ItemInfo** when a node or edge is clicked within the network.

Include **KnetMaps** in your web page's `<body>` as shown below:
```
<!-- KnetMaps -->
   <div id="knet-maps">
	<div id="itemInfo" class="infoDiv" style="display:none;"><!-- Item Info pane -->
          <table id="itemInfo_Table" class="infoTable" cellspacing=1><thead><th>Item Info:</th><th><button id="btnCloseItemInfoPane" onclick="closeItemInfoPane();">Close</button></th></thead><tbody></tbody></table>
         </div>
         <!-- KnetMaps Menubar -->
         <div id="knetmaps-menu">
              <input type="image" id="maximizeOverlay" src="image/maximizeOverlay.png" title="Toggle full screen" onclick="OnMaximizeClick();" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
              <input type="image" id="showAll" src="image/showAll.png" onclick="showAll();" title="Show all" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
              <input type="image" id="relayoutNetwork" src="image/relayoutNetwork.png" onclick="rerunLayout();" title="Relayout" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
              <span class="knet-dropdowns">
	        <select id="layouts_dropdown" class="knet-dropdowns" onChange="rerunLayout();" title="Select network layout">
                    <option value="Cose_layout" selected="selected">CoSE layout</option>
                    <option value="ngraph_force_layout">Force layout</option>
                    <option value="Circle_layout">Circular layout</option>
                    <option value="Concentric_layout">Concentric layout</option>
                    <option value="Cose_Bilkent_layout">CoSE-Bilkent layout</option>
                 </select>
                 <select id="changeLabelVisibility" class="knet-dropdowns" onChange="showHideLabels(this.value);" title="label visibility">
                     <option value="None" selected="selected">Labels: None</option>
                     <option value="Concepts">Labels: Concepts</option>
                     <option value="Relations">Labels: Relations</option>
                     <option value="Both">Labels: Both</option>
                  </select>
                 <select id="changeLabelFont" class="knet-dropdowns" onChange="changeLabelFontSize(this.value);" title="label font size">
                    <option value="12">Label size: 12px</option>
                    <option value="16" selected="selected">Label size: 16px</option>
                    <option value="20">Label size: 20px</option>
                    <option value="24">Label size: 24px</option>
                    <option value="28">Label size: 28px</option>
                    <option value="32">Label size: 32px</option>
                 </select></span>
            <input type="image" id="resetNetwork" src="image/resetNetwork.png" onclick="resetGraph();" title="Reposition" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
            <input type="image" id="savePNG" src="image/savePNG.png" onclick="exportAsImage();" title="Export png" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
            <input type="image" id="helpURL" src="image/help.png" onclick="openKnetHelpPage();" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
            <input type="image" id="saveJSON" src="image/saveJSON.png" onclick="exportAsJson();" title="Export JSON" onmouseover="onHover($(this));" onmouseout="offHover($(this));">
	</div>
        <!-- The core cytoscapeJS container -->
        <div id="cy"></div><br/>
	<div id="countsLegend"><span>KnetMaps</span></div><!-- legend -->
        <div id="infoDialog"></div><!-- popup dialog -->
  </div>
```

Now simply copy the javascript, css, libs, sampleFiles, image (for KnetMaps menu), js and config (for **ItemInfo**) folders in your web page's root directory.

By default the `js/launchNetwork.js` file works on launching a network when receiving a JSON dataset from the user. This can , modify the window.onload function code to in your simple way to have **KnetMaps** on your webpage is shown in `index.html`, i.e., using the code snippet below. This will create the KnetMaps menubar, the cytoscapeJS core container, ItemInfo panel and a legend (listing the number of total and visible nodes/ edges in the network):