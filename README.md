# BIMtoCost-Excel

## Introcuction

Automated cost calculations based on Building Information Modeling (BIM) are highly complex due to the variety in modeling styles. The model data quality depends on the architectural company's standards, each individual modeller's style, and even their daily mood.

In this project, we explore the possibilities once the issue of data variety is resolved. We utilize the abstractBIM, an automatically normalized architectural BIM, to develop a prototype for building cost calculations.

## Licence
MIT License
Copyright (c) 2023, abstract ag

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Workflow & Installation
- Download the Excel file for cost calculations.
- Replace the data in the "abstractBIM" sheet with the data from your abstracted IFC.
- To abstract an IFC model with IfcSpaces, upload it to www.abstractBIM.com and select QTO as the output.
- Download the resulting IFC and Excel files for further use.
- Replace the data in the sheet abstractBIM with your project data.

## The abstractBIM data explained
The abstractBIM tool automatically transforms any architectural BIM IFC file with IfcSpaces into a consistent data model. Either as IFC, Excel or gbXML. It achieves this by interpreting the empty spaces between the spaces as walls and slabs. This approach offers the following benefits:

- The space geometry is simplyfied.
- The following elements are consistently created:
	- Simplyfied Net Spaces as IfcSpace
	- Gross Area/Volumes (GFA) asIfcSpace:PredefinedType:GFA
	- Exterior and Interior Walls
	- Slabs with PredefinedTyp:FLOOR, BASESLAB and ROOF
	- Coverings with PredefinedTyp:CEILING, CLADDING,ROOFING and INSULATION
	- Simplyfied Windows based on the input's IfcWindows or transparent IfcCurtainWall parts
	- Simplyfied Doors based on the input's IfcDoors
	- 
- The Space, Door and Window entities keep the relation to the input's element with the Attribute ePSet_abstractBIM:originalGUIDs.
- The attributes of the entities are standardized according to the Ifc Standard's definition. This ensures consistency in quantities and other relevant data.
- Each element is aware of its relationship to the adjacent spaces. For instance, a wall knows which two spaces it separates. This relationship data enables automatic data enrichments. ePSet_abstractBIM:RelatedSpaces
- Each element is aware of its facing direction, providing valuable information for further analysis and visualization. ePSet_abstractBIM:Direction 

## The cost calculation Excel

### Sheet Overview
- abstractBIM: Lists all quantities extracted from the abstractBIM file.
- Summary: Computes fundamental quantities for future reference.
- Cost Calculation: Integrates quantities, qualities, and unit prices for cost estimation.
- Roombook: Aids in specifying room-specific qualities such as flooring.
- Wallbook: Facilitates the specification of wall-related qualities based on wall relationships between two rooms.
- Door Books: Offer detailed specifications for doors.
- Window Books: Offer detailed specifications for windows.
- SimpleBIM Wall Enrichment: Aggregates the data to bring it back to the IFC file for visualization using SimpleBIM.
- List: Contains supplementary pick lists.

### General Workflow
- Proceed through the sheets from left to right.
- Feel free to modify values in the yellow fields with confidence!

### Sheet "Cost Calculation"
The Cost Calculation sheet is the central workspace, linking all other sheets.

- Columns B and C outline the cost structure.
- Column E provides picklists for the Roombook, offering a solid starting point for your calculations.
- Column D offers a free-text field for detailed descriptions of the element's quality.
- Columns G to K permit you to input or override quantities from the abstractBIM.
- Columns M to O display the actual calculations.

### Sheet "Roombook", "Wallbook", "Facadebook", "Windowbook" and "Doorbook"

In the book Sheets you can add more information for the Spaces, Interior Walls, ExteriorCoverings. The principle is always the same. Copy the unique and insert the values (not the formulas)  of the Type names  into column B of the Book Sheet. Based on this information, the sheet calculates basic quantities, and you can add more details in the later columns.

### Sheet "SimpleBIM Wall Enrichment"
1. Copy column A with the GUID and B with the description 
2. Insert the values (not the references) into the SimpleBIM Template "SimpleBIM Enrichment Template.xlsx" in Sheet "Enrichment Walls", cell C7
3. Open the corresponding abstractBIM file with the template in SimpleBIM
4. voila, the abstractBIM IFC is enriched and you can visualize your calculation data.

### Sheet: List
Add values for picklists in the different books.

### Updating quantities

1. abstract the new model
2. Check the resul in a BIM Viewer. We recommend BIMcollab ZOOM because of the colored visualizations.
3. Copy the data from the Sheet abstractBIM. 
	- (Ctrl A) marks everything
	- (Ctrl C) copies everything
4. Insert the copied data into the existing Calculation. 
	- (Ctrl A) marks everything
	- (Delete) deletes all the data
	- Click in column A1 and insert the data with (Ctrl V) 
5. Check in the book sheets in column A and B if new types were added, if yes proceed to 6.
6. Copy the values from column B and insert the values in column A (insert values, not formulas!)
