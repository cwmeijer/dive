Please cite the software if you are using it in your scientific publication:

[![DOI](https://zenodo.org/badge/69663950.svg)](https://zenodo.org/badge/latestdoi/69663950)


# DiVE   -  Interactive Visualization of Embedded Data

 
DiVE is an interactive 3D web viewer of up to million points on one screen that represent data. It is meant to provide interaction for viewing high-dimensional data that has been previously [embedded](https://en.wikipedia.org/wiki/Nonlinear_dimensionality_reduction) in 3D. For embedding (non-linear dimensionality reduction, or manifold learning) we recommend [LargeVis](http://github.com/sonjageorgievska/LargeVis/) (a new algorithm by Microsoft Research, ) or [tSNE](https://github.com/lvdmaaten/bhtsne) .       

For an online demo click  [here](http://sonjageorgievska.github.io/DiVE/ "online demo"). Or open index.html in Mozzila Firefox to run a demo on your local computer. You need to select data from your local computer to display. Example data is in the folder *data*.   


##Installation##

The simplest way is to have the Mozilla Firefox browser installed and to open *index.html* with it.   

If you would like to use Google Chrome or any other browser, you would have to

1. Install *node.js* server together with the node package manager *npm* from (https://www.npmjs.com/get-npm).
2. Open your command line interpreter (CLI)
3. Type `npm install connect serve-static`
4. Go to the main folder of *DiVE* in your CLI (where *index.htm*l is)
5. Type `node server.js` 
7. Open your browser and type `http://localhost:8082/index.html` 

## Data description and functionality ##

* Every point has 3 coordinates and a unique ID. (For a best view, the absolute values of the coordinates should be smaller than 1. When using LargeVis with similarities (weights) as input, this can be achieved by re-scaling the similarities to be smaller than 1.) 
 
* A point also has `Categories` and `Properties`:
 
  - `Categories` is a list of strings associated with the point. This list is displayed when a user hovers over a point with the mouse or 	equivalent. The list can be empty.
  
  - `Properties` is a list of real numbers, which can be empty. Each number represents the intensity of a respective property. These 		numbers are used in the Coloring section of the UI of the web-page. When the user selects a property, and a color, every point is 	  colored with a shade of the selected color. The intensity of the color corresponds to the intensity of the selected 			property for the particular point. 

## User interaction ##
### Search ###
* A user can search for all points that contain a certain substring in their ids, names or categories, by using the *Search* section. Then all points that are a match become red, and the rest become grey. One can search also for boolean expressions of regular expressions. An example of a boolean expression is `xx AND yy OR NOT zz`, where xx, yy, and zz are regular expressions and NOT binds more than AND, which binds more than OR. In this case all points that contain in their metadata the regular espressions xx and yy, or that do not contain zz, will be coloured in red. 

* *Show only found nodes* will show only the nodes that result from the search.
  
* The *Resume colors* button at the bottom return the colors of the points to the previous coloring scheme. 

### Visualization options ###

* *Centralize*  : will move data back to center of the screen
* *Point size attenuation*: very useful when the user has zoomed-in enough. When this option is not selected, the points do not get bigger as the camera moves closer to them, so that they can be separated and inspected individually. 
* *Show point info in popup* : when de-selected, the information about a point whne hovering over it will be displayed at the top left corner of the screen rather than in a pop-up message

### Coloring by intensity of property###

As explained in section **Data description and functionality** .

## Data format ##

- The data is in a JSON (JavaScript Object Notation)  format. (See folder *data* for examples.)
To obtain *data.js*, first a data structure

		Dictionary<string, Point>

is created in any programming language, where the keys are the id’s of the points and `Point` is an object of the class 
  
		public class Point
		    {
		        public List<double> Coordinates;
		        public List<object> Categories;
		        public List<double> Properties;
		    }

`Coordinates, Categories` and `Properties` are as discussed in the previous section.

Next, the dictionary is serialized using JavaScriptSerializer and written in *data.json* (name is flexible). 
Here is an example of an entry of the serialized dictionary in a *data.json* file:

		"3951":{"Coordinates":[0.99860800383893167,0.61276015046241838,0.450976426942296],
			"Categories":["Prototheca cutis","Prototheca cutis","Prototheca","",""],
			"Properties":[9,4,4]}

Optionally, if data has numerical properties, the dictionary should also contain an entry 

		"NamesOfProperties":{"name1", "name2", "name3"}

## From output of LargeVis to input of DiVE ##
The output of [LargeVis](http://github.com/sonjageorgievska/LargeVis/) can be processed into an input of the viewer by using the python script "MakeVizDataWithProperMetaData.py" in the folder "prepareData". It is called with 
		
		python MakeVizDataWithProperMetaData.py -coord coordinatesFile -metadata metaDataFile -dir baseDir -np -namesOfPropertiesFile -pif -propertiesIntensitiesFile
		
		
		
* `coordinatesFile`: the output file of LargeVis
* `metaData`: file containing meta information about data. Format: `[id] [metadata]`.  Format of metadata:  `"first_line" "second_line" "third_line"` (number of lines is not limited). Example line of `metadata`: `35 "A dog" "Age:2" "Color brown"`.
	
* `baseDir`: base directory to store output file

* `namesOfPropertiesFile`: A json file containing list of numerical properties names. Ex: `["Pressure", "Height", "Weight"]`. If file is omitted, its name should be `"No"`
* `propertiesIntensitiesFile`: A file containing intensities of properties. File format: `[id] [intensityOfProperty1] [intensityOfProperty2]... [intensityOfPropertyN]`. Example line: `35 12.9 32.5 122.2` If file is omitted, its name should be `"No"`

## Licence ##
The software is released under the Creative Commons Attribution-NoDerivatives licence.
[Contact](mailto:s.georgievska@esciencecenter.nl) the author if you would like a version with an Apache licence. 

