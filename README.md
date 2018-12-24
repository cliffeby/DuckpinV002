# Duckpins -Project Documentation --Phase II



<img src= "https://user-images.githubusercontent.com/1431998/46451141-c32c8f80-c762-11e8-9c70-25089f44a9af.png" width = "430px" align = "left">

### _Background_
In Phase I  of Duckpins, I described the hardware, software and initial results of a project to illuminate the Lucite numbers on the headboards for Congressional Country Club’s Duckpin Bowling alleys.  Phase II is an update since October 2018 and focuses on tools for analysis, attempts to improve the streaming framerate, and alternative configurations for detecting the ball count and setter and reset actions.
### _Data Analysis Tools_
Phase I explains how nightly postprocessing of video data is analyzed and stored in an Azure table.  The data contain xy pairs of the ball locations that produce the endingPinCount.  The format of those records is:

`{'PartitionKey': 'Lane 4', 'RowKey': '20180927643118', 'beginingPinCount': 1023, 'endingPinCount': 0, 'x0': '634', 'y0': '829', 'x1': '637', 'y1': '702', >'x2': '641', 'y2': '596', 'x3': '642', 'y3': '510', 'x4': '576', 'y4': '306'}`

The data in Azure tables can be exported to Excel or PowerBi and Phase I contained a simple spreadsheet for sorting and reviewing static data.  For a more dynamic approach, I explored Azure Machine Learning Studio (AMLS) and with a little trial and error was able to create excellent visualizations and statistics.  Appendix A is a Matplotlib graphic of the top 20 endingPinCount results.

AMLS is a drag and drop interface primarily intended for creating machine learning models.  It has extensive data shaping tools and makes access to Azure data seamless.  You can also import Python scripts for greater analysis and visualization with AMLS or the python matplotlib import.  Processes can be exported to Juptyer Notebooks.  To get started in AMLS, take a look at a couple of short YouTube videos e.g. https://www.youtube.com/watch?v=csFDLUYnq4w or if you have an Azure account (free), click through the https://studio.azureml.net/Home/ Experiment Tutorial and then one of the other samples that reflect your area of interest.

### _AMLS Example_
This example imports the stored pindata from Azure Tables and shapes it into a dataset that can used to plot ball location and angle statistics for each group of pin results.  The tools take some time to adjust to.  SQLite is used in lieu of SQL and data imported for use by a Python script uses “pandas” , a data manipulation and statics import.  Syntax for both differ from their underlying base – SQL and Python, respectively. (But again, nothing that a little search and ctrl c, ctrl v can’t handle.)\
<!-- + fig 1 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384550-29ef8400-0694-11e9-9a1c-b38f8f28a3b5.png" width = "430px" align = "left">
<!-- + img 1 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361940-93a44c80-0533-11e9-8864-12b2b042ae40.png" width = "430px" align = "left">
<!-- + fig 2 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384551-29ef8400-0694-11e9-85e1-83b0e961d9fb.png" width = "430px" align = "left">
<!-- + img 2 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361947-943ce300-0533-11e9-9b08-6a6d2720d61c.png" width = "430px" align = "left">
<!-- + fig 3 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384552-29ef8400-0694-11e9-8d70-53e801ed6785.png" width = "430px" align = "left">
<!-- + img 3 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361942-93a44c80-0533-11e9-9d53-766f27f6a357.png" width = "430px" align = "left">
<!-- + fig 4 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384553-29ef8400-0694-11e9-99d2-137252c7fd0e.png" width = "430px" align = "left">
<!-- + img 4 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361944-93a44c80-0533-11e9-8f3f-a096b771e9d1.png" width = "430px" align = "left">
<!-- + fig 5 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384548-29ef8400-0694-11e9-9ce2-adbef0b054a9.png" width = "430px" align = "left">
<!-- + img 5 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361948-943ce300-0533-11e9-80cb-8e0e6179fa69.png" width = "430px" align = "left">
<!-- + fig 6 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50384549-29ef8400-0694-11e9-8011-dee51317b0ee.png" width = "430px" align = "left">
<!-- + img 6 + -->
<img src= "https://user-images.githubusercontent.com/1431998/50361939-93a44c80-0533-11e9-8dc8-bc6bb2add296.png" width = "430px" align = "left">
</br>
</br>
</br>
</br>
</br>
</br>
</br>
</br>
In Figure No. 1, I drag the Import Data object from the left menu and fill in the blanks/drop down approach or the  “Launch import Data Wizzard” on the right menu.  Once successful credentials, containers and files names are specified, the data is easily viewed and importantly can be viewed at each step in the process.  During testing, I used cached results to improve speed.
</br></br></br></br>
</br>
</br>
Figure No. 2 shows the visualized data.  Visualize is a right click option on most objects on the canvas.
</br></br></br></br></br></br></br></br></br></br>
</br>
</br>
</br>
</br>
</br>
In Figure No. 3, I use a Pthon Script to calculate a new field(column) for the data. Drag the Ptyon Script object on the canvas and connect the imported data table.  This field is the number of pins standing for each observation. The function, numsUp() simply counts the number of ones in the binary value of the endingPinCount.  In AMLS, Python Scripts import up to two pandas datasets.  These datasets are referred to as dataframe.  Adding a column is a one line statement with no need to iterate through the entire dataset. 

The Python Script is easily edited using the sript tag in the right menu.  Since AMLS is not a code editor, I recommend starting with simple Python and pandas expressions achieving results and then increasing complexity.  Error messages often terse.
```python
import pandas as pd
# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame,   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):
    def numUp():
        pcs = {}
        for pinCount in range(0, 1023):
            bits = bin(pinCount)
            count = 0
            for bit in str(bits[2:len(str(bits))]):
                if bit == '1':
                    count = count + 1
            pcs[pinCount] = count
        return pcs
    # Execution logic goes here
    print('Input pandas.DataFrame #1:\r\n\r\n{0}'.format(dataframe1))
    dataframe1['up']= dataframe1['endingPinCount'].map(numUp())
    # Return value must be of a sequence of pandas.DataFrame
    return dataframe1
```
</br>
Next in Figure No. 4, I use a SQL Transformation to calulcate velocity and approach angle of the ball from the xy coordinates in the now pandas dataframe.  The SQLite cammands also eliminate records where the value of y2 is null.

```sql
select endingPinCount as epc,up,
        SQRT(SQUARE(x1-x0)+SQUARE(y1-y0)) as v1,
        SQRT(SQUARE(x2-x1)+SQUARE(y2-y1)) as v2,
        ATAN(CAST((x2-x1) as float)/CAST((y2-y1) as float)) as theta,
        CAST(x1 as float) as x
                        --CAST(x1 as abs(float(x)-562)) as absx
                       -- ATAN(CAST((x3-x2) as float)/CAST((y3-y2) as float)) as angle3
from t1
WHERE y2 IS NOT NULL;
```
Finally in Figure No. 6, the data has been shaped to contain the ending PinCount, two speed calculations, the angle of approach and the x location of the ball.  Plotting this data using the Python library matplotlib, assures that the data collection and analysis process is as expected.  The python script for plotting the pins, ball location, distribution, and angle is unique to this application.  I have included it in Appendix B.

To produce Appendix A, I did not use AMLS.  A straigh Python script provided more flexibility to use subplots and embedded tables.  This code is included as Appendix C.

Plots created using MatplotLib that can be visualized on the browser can be returned by the Execute Python Script. But the plots are not automatically redirected to images as they are when using R. So the user must explicitly save any plots to PNG files if they are to be returned back to Azure Machine Learning.

To generate images from MatplotLib, you must complete the following procedure:

switch the backend to “AGG” from the default Qt-based renderer
create a new figure object
get the axis and generate all plots into it
save the figure to a PNG file
This process is illustrated in the following Figure 8 that creates a scatter plot matrix using the scatter_matrix function in Pandas.

#### _Duckpins_ #
