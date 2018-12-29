# Duckpins -Project Documentation --Phase II

Note to reader - The Dinky styling for this page is not easily readable.  I suggest that you read on GitHub by clicking the "View on GitHub" in the adjacent panel.         --Cliff Eby Dec 2018

<img src= "https://user-images.githubusercontent.com/1431998/46451141-c32c8f80-c762-11e8-9c70-25089f44a9af.png" width = "430px" align = "left">

### _Background_
In Phase I  of Duckpins, I described the hardware, software and initial results of a project to illuminate the Lucite numbers on the headboards for Congressional Country Club’s duckpin bowling alleys.  Phase II is an update to the project and focuses on tools for analysis, improvement to the streaming framerate, and alternative configurations for detecting the ball count and setter and reset actions.

### _Data Analysis Tools_
Phase I explains how nightly postprocessing of video data is analyzed and stored in an Azure table.  The json data contain xy pairs of the ball locations that produce the endingPinCount.  The format of those records is:

`{'PartitionKey': 'Lane 4', 'RowKey': '20180927643118', 'beginingPinCount': 1023, 'endingPinCount': 0, 'x0': '634', 'y0': '829', 'x1': '637', 'y1': '702', >'x2': '641', 'y2': '596', 'x3': '642', 'y3': '510', 'x4': '576', 'y4': '306'}`

The data in Azure tables can be exported to Excel or PowerBi and Phase I contained a simple spreadsheet for sorting and reviewing static data.  For a more dynamic approach, I explored Azure Machine Learning Studio (AMLS) and with a little trial and error was able to create excellent visualizations and statistics.  Appendix A is a Matplotlib graphic of the top 20 endingPinCount results from the Azure table data.

AMLS is a drag and drop interface primarily intended for creating machine learning models.  It has extensive data shaping tools and makes access to Azure data seamless.  You can also import Python scripts for greater analysis.  Data visualization with AMLS or the python matplotlib import is supported and processes can be exported to Juptyer Notebooks.  To get started in AMLS, take a look at a couple of short YouTube videos e.g. https://www.youtube.com/watch?v=csFDLUYnq4w or if you have an Azure account (free), click through the https://studio.azureml.net/Home/ Experiment Tutorial and then one of the other samples that reflect your area of interest.

### _AMLS Visualization Example_
This example imports the stored pindata from Azure Tables and shapes it into a dataset that can used to plot ball location and angle statistics for each group of pin results.  The tools take some time to adjust to.  SQLite is used in lieu of SQL and data imported for use by a Python script uses “Pandas” , a data manipulation and statistics import.  Syntax for both differ from their underlying base – SQL and Python, respectively. (But again, nothing that a little search and ctrl c, ctrl v can’t handle.)

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
</br>
</br>
</br>
</br>
</br>
</br>
Figure No. 2 shows the visualized data.  Visualize is a right click option on most objects on the canvas.
</br></br></br></br></br></br>
</br>
</br>
</br>
</br>
</br>
In Figure No. 3, I use a Pthon Script to create a new field(column) for the data. Drag the Pthyon Script object on the canvas and connect it to the imported data table.  This new field is the number of pins standing for each record. The function, numsUp() simply counts the number of ones in the binary value of the endingPinCount.  In AMLS, Python Scripts import up to two pandas datasets.  These datasets are referred to as dataframes.  Adding a column is a one line statement with no need to iterate through the entire dataset. 

The Python Script is easily edited using the sript tag in the right menu.  Since AMLS is not a code editor, I recommend starting with simple Python and pandas expressions achieving results and then increasing complexity.  AMLS error messages often terse.
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
Next in Figure No. 4, I use a SQL Transformation to calulcate velocity and approach angle of the ball from the xy coordinates in the now pandas dataframe.  The SQLite WHERE command also eliminates records where the value of y2 is null.

```sql
select endingPinCount as epc,up,
        SQRT(SQUARE(x1-x0)+SQUARE(y1-y0)) as v1,
        SQRT(SQUARE(x2-x1)+SQUARE(y2-y1)) as v2,
        ATAN(CAST((x2-x1) as float)/CAST((y2-y1) as float)) as theta,
        CAST(x1 as float) as x
from t1
WHERE y2 IS NOT NULL;
```
Finally in Figures No. 5 & 6, the data has been shaped to contain the ending PinCount, two speed calculations, the angle of approach and the x location of the ball.  Plotting this data using the Python library matplotlib, assures that the data collection and analysis process are as expected.  The python script for plotting the pins, ball location, distribution, and angle is unique to this application.  I have included it in Appendix B.

To produce Appendix A, I did not use AMLS.  A straight Python script provided more flexibility to use subplots and embedded tables.  This code is included as Appendix C.

</br>
</br>
</br>
</br>
</br>

### _AMLS Analytics Sample_
A question posed in Phase I was, "How does ball speed affect score?"  One approach is to compare speed to the number of pins remaining after a ball is thrown at 10 pins.  Using the dataset shaped above, I grouped the data by pinsUp and plotted the speed of the ball.  
<img src ="https://user-images.githubusercontent.com/1431998/50408064-4386ed80-07b2-11e9-955b-2b77e9c08f58.png" width = "430px" align = "left">
Figure No. 7 shows the result and it appears that there is slight advantage to faster ball speeds. The plot shows that the average speed is highest for strikes and then 9s. Scores of 8 through 4 also reflect the benefit of ball speed.  Only when scores are 3, 2 and 1 does the data show the benefit of a slower roll.

A similar analysis could be performed for the ball angle.
</br></br></br></br></br></br>

### _Jupyter Notebook_
https://notebooks.azure.com/clifford-eby/projects/duckpinphase2



Jupyter Notebooks
Jupyter Notebooks, formerly iPython, is typically referred to as Jupyter. With the beta release of Jupyter Labs, an IDE version, I will refer to just the notebook as JN to avoid confusion for future readers.

JN is a combination of Markdown with executable code in cells. This example will repeat much of what was achieved above using AMLS. The text below is HTML, but the complete JN can be cloned from my GitHub repo.

For a good description of JN on Azure, Scott Hanselman has a nice YouTube video on JN many features - https://www.youtube.com/watch?v=JWEhns28Cr4

Azure Storage SDK
Surprisingly, the Azure Storage SDK was not pre-installed in Azure hosted JN. Use pip or Annaconda (package managers) to install the azure storage sdk. Once installed, this shell/bash command can be eliminated for futre runs in this project.

In [ ]:
!pip install azure-storage
Import Data
Azure Table data is imported using the Azure Storage /Table Storage SDK and the data is shaped using Pandas to match the AMLS dataset. I would typically import my credentials which are needed for Azure access, but for this public notebook, I use a Shared Access Signature token to access the data in read/query only format. The token has an expiration date which can be refresehed on request. Alternatively, I have a small sample dataset, in my DuckpinA repo on GitHub. See below in Data Alternatives on how to access web based data.

Imports and constants
In [1]:
# import credentials
from azure.storage.table import TableService
import numpy as np
import pandas as pd

container_name = 'duckpinjson'
account_name = container_name
# account_key = credentials.STORAGE_ACCOUNT_KEY
table_sas_token = 'sp=r&sv=2017-04-17&tn=pindata&sig=0vcZimSsLnyez5Kw5tOt6JASJ4MrVCnz13cOySaxtJQ%3D&se=2019-01-29T11%3A54%3A17Z'
table_name = 'pindata'
Get Table Data and convert to Pandas Dataframe
In [2]:
# Two fancy Python functions to iterate Azure Tables and persist the result in a Pandas dataframe  
def get_dataframe_from_table_storage_table(table_service, filter_query):
    """ Create a dataframe from table storage data """
    return pd.DataFrame(get_data_from_table_storage_table(table_service,
                                                          filter_query))
def get_data_from_table_storage_table(table_service, filter_query):
    """ Retrieve data from Table Storage """
    for record in table_service.query_entities(
        table_name, filter=filter_query
    ):
        yield record
        
# table_service = TableService(
#     account_name=account_name, account_key=account_key)
table_service = TableService(
    account_name=account_name, sas_token=table_sas_token)
fq = "PartitionKey eq 'Lane 4'"
df = get_dataframe_from_table_storage_table(table_service=table_service,filter_query=fq)
df.head(5) #Show first five records in the dataframe
Out[2]:
PartitionKey	RowKey	Timestamp	beginingPinCount	endingPinCount	etag	res	x0	x1	x2	x3	x4	x5	y0	y1	y2	y3	y4	y5
0	Lane 4	20181019044781	2018-10-19 23:23:18.239444+00:00	1023	876	W/"datetime'2018-10-19T23%3A23%3A18.2394448Z'"	NaN	763	760	754	754	755	NaN	388	319	229	158	97	NaN
1	Lane 4	20181019047495	2018-10-19 23:01:57.236804+00:00	1023	947	W/"datetime'2018-10-19T23%3A01%3A57.2368045Z'"	NaN	225	244	259	274	290	NaN	397	363	305	245	186	NaN
2	Lane 4	20181019054037	2018-10-19 23:01:48.233574+00:00	1023	930	W/"datetime'2018-10-19T23%3A01%3A48.2335741Z'"	NaN	1006	974	948	NaN	NaN	NaN	314	173	61	NaN	NaN	NaN
3	Lane 4	20181019059314	2018-10-19 22:59:22.238739+00:00	1023	1007	W/"datetime'2018-10-19T22%3A59%3A22.2387392Z'"	NaN	870	861	853	849	840	NaN	384	271	166	80	15	NaN
4	Lane 4	20181019075651	2018-10-19 23:00:39.253765+00:00	1023	661	W/"datetime'2018-10-19T23%3A00%3A39.2537652Z'"	NaN	358	373	383	392	400	NaN	421	389	349	289	232	NaN
In [3]:
df.tail(5) #Show last five records in the dataframe
Out[3]:
PartitionKey	RowKey	Timestamp	beginingPinCount	endingPinCount	etag	res	x0	x1	x2	x3	x4	x5	y0	y1	y2	y3	y4	y5
3652	Lane 4	20181221856778	2018-12-22 03:48:29.726076+00:00	1023	643	W/"datetime'2018-12-22T03%3A48%3A29.7260768Z'"	1440	399	413	425	436	446	456	392	337	256	185	122	68
3653	Lane 4	20181221916380	2018-12-22 03:49:08.792957+00:00	1023	678	W/"datetime'2018-12-22T03%3A49%3A08.7929576Z'"	1440	199	240	275	306	335	360	398	330	243	167	101	41
3654	Lane 4	20181221964908	2018-12-22 03:46:42.825063+00:00	1023	1022	W/"datetime'2018-12-22T03%3A46%3A42.8250631Z'"	1440	1136	1102	1075	1049	1030	1016	395	325	231	154	88	31
3655	Lane 4	20181221980028	2018-12-22 03:51:20.847274+00:00	1023	959	W/"datetime'2018-12-22T03%3A51%3A20.8472743Z'"	1440	128	167	199	228	253	278	391	333	264	202	143	91
3656	Lane 4	20181221996955	2018-12-22 03:51:04.859007+00:00	1023	951	W/"datetime'2018-12-22T03%3A51%3A04.8590072Z'"	1440	216	250	271	291	307	325	415	370	297	225	159	104
Compute the number of pins standing for each decimal value of the endingPinCount
In [4]:
#Function to count the number of 1s in the binary equivalent of a decimal value
# Result returned in a dictionary for all pin configurations
def numUp():
    pcs = {}
    for pinCount in range(0, 1023):
        bits = bin(pinCount)
        count = 0
        # print(bits, str(bits))
        for bit in str(bits[2:len(str(bits))]):
            if bit == '1':
                count = count + 1
            # print(bit, count)
        pcs[pinCount] = count
    return pcs
# Add up column to dataframe
df['up']= df['endingPinCount'].map(numUp())
df.head(5)
Out[4]:
PartitionKey	RowKey	Timestamp	beginingPinCount	endingPinCount	etag	res	x0	x1	x2	x3	x4	x5	y0	y1	y2	y3	y4	y5	up
0	Lane 4	20181019044781	2018-10-19 23:23:18.239444+00:00	1023	876	W/"datetime'2018-10-19T23%3A23%3A18.2394448Z'"	NaN	763	760	754	754	755	NaN	388	319	229	158	97	NaN	6
1	Lane 4	20181019047495	2018-10-19 23:01:57.236804+00:00	1023	947	W/"datetime'2018-10-19T23%3A01%3A57.2368045Z'"	NaN	225	244	259	274	290	NaN	397	363	305	245	186	NaN	7
2	Lane 4	20181019054037	2018-10-19 23:01:48.233574+00:00	1023	930	W/"datetime'2018-10-19T23%3A01%3A48.2335741Z'"	NaN	1006	974	948	NaN	NaN	NaN	314	173	61	NaN	NaN	NaN	5
3	Lane 4	20181019059314	2018-10-19 22:59:22.238739+00:00	1023	1007	W/"datetime'2018-10-19T22%3A59%3A22.2387392Z'"	NaN	870	861	853	849	840	NaN	384	271	166	80	15	NaN	9
4	Lane 4	20181019075651	2018-10-19 23:00:39.253765+00:00	1023	661	W/"datetime'2018-10-19T23%3A00%3A39.2537652Z'"	NaN	358	373	383	392	400	NaN	421	389	349	289	232	NaN	5
Shape with Pandas and exclude unwanted columns.
Use Pandas to generate the dataset created in AMLS with SQLite. The sql was:

select endingPinCount as epc,
        up,y1, 
        SQRT(SQUARE(x1-x0)+SQUARE(y1-y0)) as v1,
        SQRT(SQUARE(x2-x1)+SQUARE(y2-y1)) as v2,
        ATAN(CAST((x2-x1) as float)/CAST((y2-y1) as float)) as theta,
        CAST(x1 as float) as x
from t1
WHERE y2 IS NOT NULL;
If you are following closely, you will see that I combined AMLS sequences to get theta and x to absolute values abst and absx, respectively.

In [5]:
df1 = df
df1['v1'] = np.sqrt((df['x1'].astype(float)-df['x0'].astype(float))**2 + (df['y1'].astype(float)-df['y0'].astype(float))**2)
df1['v2'] = np.sqrt((df['x2'].astype(float)-df['x1'].astype(float))**2 + (df['y2'].astype(float)-df['y1'].astype(float))**2)
df1['abst'] = np.arctan((df['x2'].astype(float)-df['x1'].astype(float))/ (df['y2'].astype(float)-df['y1'].astype(float))).abs()
df1 = df1[df1["y1"].notnull()]
df1['absx'] = (df1['x1'].astype(float)-562).abs()
df1= df1.drop(['PartitionKey','RowKey','Timestamp','beginingPinCount','etag', 'res',
               'x0','y0','x1','y1','x2','y2','x3','y3','x4','y4','x5','y5'], axis=1)
df1.head(5)
Out[5]:
endingPinCount	up	v1	v2	abst	absx
0	876	6	69.065187	90.199778	0.066568	198.0
1	947	7	38.948684	59.908263	0.253076	318.0
2	930	5	144.585615	114.978259	0.228103	412.0
3	1007	9	113.357840	105.304321	0.076044	299.0
4	661	5	35.341194	41.231056	0.244979	189.0
Similar to AMLS, I plot the data but this time will use theta to see if ball angle and presumably spin affect pin results. A shell/bash command is needed to redirect the plot output to JN.

In [6]:
%matplotlib inline
In [7]:
import matplotlib.pyplot as plt

fig =plt.figure() 
ax = fig.gca()
ax.cla()
ax.set_xlim(-1, 10)
ax.set_ylim(-1,1)
ax.set_title('Does ball angle/spin matter?')
plt.ylabel('mean angley and deviation')
plt.xlabel('# of pins remaining after roll')
for x in range(0,10):
    #pandas syntax to group data by the number of up pins.
    endcountGroup = df1.loc[df1['up'] == x]
    #pandas describe()generates a table of stats for the dataframe
    g = endcountGroup.describe()
    ax.errorbar(x, g.iloc[1][4], g.iloc[2][4], marker='^')
# fig.savefig ("duckpin.png")

### _Alternative Data Souces_ #### Github Repo In my GitHub repo, I have a dataset (pindatacsv.csv) that can be used for this notebook in lieu of the full dataset using the SAS token above. I also have a dataset (results01.csv) used for testing. It is 3400+ records of csv data in the format: `epc,up,y1,v1,v2,theta,x` To access that data, use curl and the url for the RAW option for that file in my DuckpinA repo.
In [8]:
!curl https://raw.githubusercontent.com/cliffeby/DuckpinA/master/pindatacsv.csv -o pincsv.csv
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  323k  100  323k    0     0  1403k      0 --:--:-- --:--:-- --:--:-- 1411k
Use numpy or pandas to read the file. In this case, I used numpy and sorted the records prior to creating the dataframe

In [9]:
csv = np.recfromcsv('pincsv.csv')
csv = np.sort(csv)
df3 = pd.DataFrame(csv)
df3.head(5)
Out[9]:
partitionkey	rowkey	timestamp	beginingpincount	endingpincount	x0	y0	x1	y1	x2	y2	x3	y3	x4	y4	up
0	b'Lane 4'	20181019044781	b'10/19/2018 11:23:18 PM'	1023	876	763	388	b'760'	b'319'	b'754'	b'229'	b'754'	b'158'	b'755'	b'97'	6
1	b'Lane 4'	20181019047495	b'10/19/2018 11:01:57 PM'	1023	947	225	397	b'244'	b'363'	b'259'	b'305'	b'274'	b'245'	b'290'	b'186'	7
2	b'Lane 4'	20181019054037	b'10/19/2018 11:01:48 PM'	1023	930	1006	314	b'974'	b'173'	b'948'	b'61'	b'""'	b'""'	b'""'	b'""'	5
3	b'Lane 4'	20181019059314	b'10/19/2018 10:59:22 PM'	1023	1007	870	384	b'861'	b'271'	b'853'	b'166'	b'849'	b'80'	b'840'	b'15'	9
4	b'Lane 4'	20181019075651	b'10/19/2018 11:00:39 PM'	1023	661	358	421	b'373'	b'389'	b'383'	b'349'	b'392'	b'289'	b'400'	b'232'	5
Creating the SAS token
The following shows how to create a Shared Access Signature to access the Azure table data in read/query only format. Since the my credentials are needed to create the token, I have moved the code to a Markdown cell. To create the sas_token:

from azure.storage.table.models import TablePermissions
from datetime import datetime, timedelta

table_service = TableService(
    account_name=account_name, account_key = account_key)
table_sas_token = table_service.generate_table_shared_access_signature(
    table_name, 
    TablePermissions.QUERY, 
    datetime.utcnow() + timedelta(hours=800))

table_sas_token



### _Improving Framerates_

### _Hardware Updates_
#### GPIO Breakout Kit
As GPIO pin use grew to support the 7-segment ball indicator and the input from the laser tripwire to detect the ball, it became difficult to adjust the wiring and to keep the jumper wires tightly inserted in the RPI, relay, and sensor boards.  Breakout kits povide some flexibility but but require soldering to a PCB board and managing the maze of jumper wires is not improved.  I'm current using the breakout board, but may go back to the direct RPI connection and change GPIO pin numbers to keep as much of the ribbon unseparated as possible.

#### Photoresistor modules
Digital output from a photoresistor was used by the laser tripwire to detect ball movement.  I found that the tripwire's sensitivity could detect all balls, but that the Python script running on the RPI was not able to reliable read the HIGH input value on the GPIO pin.  When it was performing other activities (finding pins or movement with the reset arm or setter), it often missed the HIGH state and would not count the tripping event.
<img src ="https://user-images.githubusercontent.com/1431998/50408086-a7111b00-07b2-11e9-9457-481a062cc2db.jpg" width = "200px" align = "left">
To solve this issue, I plan to try a Bistable latching relay module.  This will "save" the HIGH state of the output pin until the RPI is ready to read it.  At that point, it gets reset to LOW and the ball counter is incremented.  This relay also solves a voltage issue as the photoresistor module requires 5V to operate and outputs 5V to the RPI.  The recommended max to an RPI GPIO input is 3.3V.

An alternative to the reset arm and setter detection routines is also being considered.  Both the reset arm and setter (deadwood) actions are user initiated by buttons at the head of the lane.  Both actions turn on lights on the headboard that stay lighted during the reset/deadwood action.  Using a photoresistor module for each would eliminate the computational effort to dected arm and setter movement and the light cycle is long enough to avoid the need for the latching relay.   


### _Appendix A - Plots of high-frequency results_

![figure_1-1](https://user-images.githubusercontent.com/1431998/50408212-bee99e80-07b4-11e9-9aeb-db3362c42fd3.png)
![figure_2-1](https://user-images.githubusercontent.com/1431998/50408213-bee99e80-07b4-11e9-892e-0556dd4522bc.png)
![figure_3-1](https://user-images.githubusercontent.com/1431998/50408214-bee99e80-07b4-11e9-881d-b93e8e9ce015.png)
![figure_4-1](https://user-images.githubusercontent.com/1431998/50408215-bee99e80-07b4-11e9-8f2d-2cf1304e4da1.png)
![figure_5](https://user-images.githubusercontent.com/1431998/50408082-9c568600-07b2-11e9-8c9e-b0653371eea0.png)

### _Appendix B AMLS python script for plotting a figure_

Plots created using MatplotLib in AMLS are not automatically redirected to images. The user must explicitly save any plots to PNG files.

To generate images from MatplotLib, use the following process:
* switch the backend to “AGG” from the default renderer
* create a new figure object
* get the axis and generate all plots into it
* save the figure to a PNG file

```python
import pandas as pd
import math
import matplotlib
matplotlib.use ('agg' ) #Change backend to PNG
import matplotlib.pyplot as plt

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame -format is  up, v1, v2, theta, x  
#   Param<dataframe2>: a pandas.DataFrame -not used
def azureml_main(dataframe1 = None, dataframe2 = None):

    fig =plt.figure() 
    ax = fig.gca()
    ax.cla()
    ax.set_xlim(-1, 10)
    ax.set_ylim(0, 300)
    ax.set_title('Does ball speed matter?')
    plt.ylabel('mean velocity and deviation')
    plt.xlabel('# of pins remaining after roll')
    for x in range(0,10):
        endcountGroup = dataframe1.loc[dataframe1['up'] == x]
        g = endcountGroup.describe()
        ax.errorbar(x, g.iloc[1][1], g.iloc[2][1], marker='^')
        # print('data', x, g.iloc[1][1])
    fig.savefig ("duckpin.png")
    return dataframe1,
```
### _Appendix C - Python script for using Azure data to plot statistics for high-frequncy results_

```python
import credentials
from azure.storage.blob import BlockBlobService
import numpy as np
import collections
import time
import pandas, math
from collections import defaultdict, OrderedDict
import matplotlib.pyplot as plt
from operator import itemgetter

container_name = 'dpanalysis'
account_name = credentials.STORAGE_ACCOUNT_NAME
account_key = credentials.STORAGE_ACCOUNT_KEY
file_name = 'results02.csv'

# Determine which pins are up(filled) or down(unfilled)
# pinCount is decimal value of 1111111111 = 1023 or 0000000000 = 0
# A pin value of 1 is up and 0 is down.
# Pin 1 = decimal 512; pin 10 = decimal 1
# bit_fill returns True when pin is up(filled)
def bit_fill(pin, pinCount):
    bits = "{0:b}".format(pinCount)
    while len(bits) < 10:
        bits = "0"+bits
    if(bits[pin] == "1"):
        return True
    else:
        return False
      
def drawPins(endingPinCount, index, value, g):
    # xy pairs for pins on figure
    pinxy = [(570, 200), (405, 300), (725, 300), (240, 400), (570, 400),
             (880, 400), (80, 500), (400, 500), (720, 500), (1040, 500)]
    circle_size = 40 # pin size on figure
    center = 570  # xy  coordinates are x from the left lane edge.  center is xlocation in pixels
    # Subplots on figure in 2 x 2 grid
    plt.subplot2grid((2,2),(int((index/2)%2),index%2))
    # create axes for subplots
    ax = plt.gca()
    ax.cla()
    # ax.set_xlim(20, 1310)
    ax.set_xlim(-center,center)
    ax.set_ylim(0, 800)
    offset = math.tan(math.radians(g.iloc[1][4]))*400

    # plot ball path on figure
    ax.plot([round(g.iloc[5][5]-center,0),round(g.iloc[5][5]-center+offset,0)],[0,400],'--')
    # plot x location of ball.  Size is +- one standard deviation
    ax.scatter(round(g.iloc[5][5]-center,10),0,s=2*round(g.iloc[2][5],0))
    # create table and entries
    #format of grouped.describe() 'g' - use g.iloc[row][col]
#          epc    up          v1          v2      theta           x         absx
# count   224.0  224.0  224.000000  224.000000  224.000000  224.000000  224.000000
# mean   1015.0    9.0  108.776786   99.799016   -0.191964   89.531250  472.468750
# std       0.0    0.0   47.187957   33.526380    0.394727   45.949172   45.949172
# min    1015.0    9.0   10.000000   10.816654   -1.000000   11.000000  321.000000
# 25%    1015.0    9.0   76.750000   81.300756    0.000000   52.000000  443.000000
# 50%    1015.0    9.0  111.500000  105.181259    0.000000   85.000000  477.000000  -- median
# 75%    1015.0    9.0  138.250000  121.371143    0.000000  119.000000  510.000000
# max    1015.0    9.0  268.000000  168.324092    0.000000  241.000000  551.000000
    cells =[[int(g.iloc[1][5]-center),int(g.iloc[1][2]),int(g.iloc[1][3]),round(g.iloc[1][4],2)],              #Row 1
            [round(g.iloc[5][5]-center,0),round(g.iloc[5][2],0),round(g.iloc[5][3],0),round(g.iloc[5][4],2)],  #Row 5
            [round(g.iloc[2][5],0),round(g.iloc[2][2],0),round(g.iloc[2][3],0),round(g.iloc[2][4],2)]]  #Row 2
    cols = ['x', 'v1','v2',r'$\theta$' ]
    rows = ['mean', 'median',r'$\sigma$' ]
    # plot filled/unfilled pin circles, title, table, and population on figure
    for idx in range(0, len(pinxy)):
        circle = plt.Circle((pinxy[idx][0]-center, pinxy[idx][1]), circle_size,
                            color='b', fill=bit_fill(idx, endingPinCount))
        plt.gcf().gca().add_artist(circle)
    plt.title('Lane 4 ' + str(endingPinCount))
    plt.table(cellText=cells,
                      rowLabels=rows,
                      colLabels=cols,
                      loc='bottom',
                      bbox=[.2, -.75, .75, 0.5])
    plt.text(-100, 700, 'pop = ' +str(value))

#  Sorts the data dictionary by desired order
#  type = 'Count' sorts by most frequent
#  type = 'Up' sorts by best result
def arrangeDict(d,type):
    if type == 'Count' or type == '':
        # type = result
        d_sorted_keys = sorted(d, key=d.get,reverse=True)
        arranged = {}
        for r in d_sorted_keys:
            arranged[r] = d[r]
        return arranged
    if type == 'Up':
        #TODO
        return d    

# Get CSV data fro Blob storage - Persist in this directory with same name as in Blob Storage
block_blob_service = BlockBlobService(
    account_name=account_name, account_key=account_key)
block_blob_service.get_blob_to_path(container_name, file_name, file_name)
# file_name = 'small.csv'  #Testing data subset - small
# file_name = 'results01.csv' #Testing data subset - large
# Format is epc,up,v1,v2,theta,x, absx
csv = np.recfromcsv(file_name)
csv1 = np.sort(csv)
endCount = []
for row in csv1:
    endCount.append(row[0])
unique, counts = np.unique(endCount, return_counts=True)
ecDict = dict(zip(unique, counts))
print('Dictionary length: ',len(ecDict))
print('First three dict pairs', {k: ecDict[k] for k in list(ecDict)[:3]})
print('Final dict pairs', {k: ecDict[k] for k in list(ecDict)[(len(ecDict)-4):len(ecDict)]})

result = pandas.DataFrame(csv1)
index = 0
for (key, value) in sorted(arrangeDict(ecDict,'Count').items(), key=itemgetter(1), reverse=True):
    if index%4 ==0:
        plt.tight_layout(pad=4.0, w_pad=0.5, h_pad=6.0)
        plt.figure(index/4 +1)
        plt.suptitle('Page '+str(index//4+1) +'  '+time.strftime('%b %d %Y'))
    if index > 19:
        continue
    endcountGroup = result.loc[result['epc'] == key]
    if index%10 ==0:
        print('Stats for group ',index, endcountGroup.describe())     
    drawPins(key, index, value,endcountGroup.describe())
    index = index+1

plt.tight_layout(pad=4.0, w_pad=0.5, h_pad=6.0)
plt.show()
```
