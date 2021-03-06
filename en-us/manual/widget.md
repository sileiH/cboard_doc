<h1> Widget </h1>

---

** Widget -&gt; new-&gt; Datasource/Dataset (Drop-down Selection)**

## Load Data

### Load from existing dataset
If you load data from existing datasets, you can use the pre-defined `Calculated Measures`, `Filter`, `schema tree` and other features of `Dataset`. <mark> It's not allowed to create a new `Calculated Measure` on a chart design page when you use datasource create widget! </mark>

### Load from direct from datasource
The default way to <kdb>load data</kbd> is to read from the existing `dataset`. You can switch load data type and write query script to experience `Type SQL, Get Chart`

### Read data from Saiku2.x

Input the Repository of your `Saiku` as below

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Saiku Repository</th>
            <th>CBoard Saiku Query</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><img src="/assets/Saiku_Rep.jpg" class="img-responsive" alt="Image"></td>
            <td><img src="/assets/saikuquery.jpg" class="img-responsive" alt="Image"></td>
        </tr>
    </tbody>
</table>

## Design Widget
<div class="t-alien-center">
    <img src="/assets/config_widget.png" alt="Image">
    <p>Widget Design Page</p>
</div>

### Page districts Introduction
#### Chart list Area
* List saved charts
* The upper-left function keys allow you to create, copy, edit or delete a chart. You need to select a single chart first, except creating a new chart.
* Delete can only apply on a single chart
* The directory tree structure is automatic generated by path name rule as the \[Folder\]?/\[SubFolder\]\*/\[chart name\]
* Folder is virtually based on file name
* ** Double-click chart ** to edit
* Select a chart and press **Delete** button on keyboard is short cut to delete a chart

#### Query Area
* The query area defaults to select the existing `datasets(Cube)`
* The bottom two function buttons are switch query mode (Switching the query between the dataset and the new AD-Hoc query) and `Load data`(JDBC offline data source data is loaded into the server cache, whereas the DataSource that supports and been setting to DataSource aggregation will only query for schemas)
* `Current operation status`
    * `New` Status: every `save` action will create a new chart, which will facilitate to continuous design of multiple charts with different granularity and different presentation patterns.
    * **Attention **:If you do not need a chart name in a new state, clicking Save in succession will prompt you for a rename error
    * ** Edit \(Edit\)**Status: modify to save to affect the current report, you can click Save again

!> In `New` status, if you don't change the filename when click `Save` button again, you will see error info of <mark>duplicate name</mark>

#### Model area
* The hierarchy tree in the model area needs predefine in `dataset design page`, otherwise you can only see a flat selects list
* The model contains four parent nodes
    * `Dimension`
        * The columns below the hierarchy can be drilled / rolled under the cross table model
    * `Measures`
    * `Calculated Measures`: need Pre-defined, can only be used, can not be modified
    * `Filter`

![](/assets/drill_table.gif)


#### Design area
* Chart name format: `[Folder]?/[SubFolder]*/[ChartName]`
* Switch chart type: different types of charts have different restrictions on input, hover on the chart type ICON to check restriction
* Drag and Drop for OLAP and design

| Design area target box | Model area node type |
| :--- | :--- |
| `Row` box , `Column dimension`、`Filter` | `Dimension` node |
| `Value ` Box | `Measure` node, `Calculated Measure` node |
| `Filter` Box | `Dimension` node, `Filter` node |

* Function button
    * Preview
    * Preview query: view the dynamic query script for data source aggregation query
    * Save
    * Cancel: revert design change


#### Preview Area
* `Preview`: Preview widget
* `Query`: Debug query script
* `Option`: Chart Setting

### Fundamental★

In theory, multi-dimensional analysis is based on multi-dimensional data, multiple sequences <kbd>series</kbd> share the same index, the most intuitive representation of the data is the `cross table`

| Cross table composition | Chart design field |
| :--- | :--- |
| Row Header | Nodes in a row |
| Column Header | Nodes in `Column` Combined `Measure` in Value box |
| Aggregate data area | Aggregate function apply on data set |

* As shown in the figure below, the header merge of the can only be correct when the header is sorted correctly

!> Please make sure you have get what's cross table and how to design a cross table before Go Ahead!


![](/assets/cross_table.png)

### Slice and Spice

* The `node` placed in the `Row` box and `Column` box can be filtered by clicking the edit button
* You can also place dimensions in the `Filter` box to filter data purely

!> Make sure you are not loading a large size list or continuous value before load dimension members!

![](/assets/filter.png)
![](/assets/Range_Filter.png)
* <mark>Filtering comparisons only support string and numeric comparison</mark>, because time functions differ from database to database, but most database `datetime` data type can be compared to a standard date string. Some databases may have index invalidation due to data type conversion. As of now, there is no special processing on datatime dimension here, please do some pre-processing on this piece while the data is ready, and debug with preview query at all times.
* Columns on values can also be sorted and filtered
* Support for input range comparison
* TOP N display

!> As you can see from the form of the `cross table`, there is a sort conflict between the value sort and the row header sort, and the row sort has only one sort of value and only one is in force. **


### Basic Graphics
#### Bar/Line Chart

One column for one line for a series bar.
![](/assets/line_bar.png)


| Design District | Chart | requirement |
| :--- | :--- | :--- |
| Row Dimension | x-axis | Place one or more dimension nodes |
| Column Dimension | category | Place zero or more dimension nodes |
| Value | | Place one or more metric nodes \(** Please don't ask us repeatedly why we don't have graphics when we don't have indicators **\) |
| Addition axis | Display double axis | It is recommended that different show type axes be configured |
| Vertical / Horizontal | X/Y axis position exchange | - |



#### Bar chart

One column for a pie chart

![](/assets/chart_pie.png)

#### KPI card

KPI card needs no dimensional information, only one measure can be selected. formatted Formtter reference [numbro api](http://numbrojs.com/format.html)

#### Funnel chart

One row for a funnel.

Normally, a funnel needs to be displayed such as `display -> click -> submit -> pay`, a series of values for different measures, with multiple columns placed in the cross table for the value axis
The values in the row are sorted automatically by size to form a funnel.
** So a row of funnels, a funnel. **, The following Demo makes no real sense, just as a demonstration

![](/assets/chart_funnel.png)

#### Sanky chart

![](/assets/Sanky.png)

Use row header and column header as nodes and value in data area as line weight. Crosstable can be considered as a joint-matrix for Sanky chart.

<div class="bs-callout bs-callout-warning" id="callout-focus-demo">
    <h4>Why does my Sankey Diagram have no hierarchy?</h4>
    <p>A lot of people ask why there is no hierarchy in the Sankey Chart. The answer is that the hierarchy of the Sankey is related to your data itself.</p>
    Data include links `A->B and B->C`,  then `B` will be a intermediate layer.
    Note also that ECharts requires data and cannot be looped \(A-&gt;B..-&gt;A\)
</div>

#### Radar

One column for one circle on Radar.

![](/assets/Chart_Radar.png)

#### Bubble diagram

| Design District | chart | requirement |
| :--- | :--- | :--- |
| Row dimension | x-axis | Place one or more dimension nodes |
| Column | classify | Place zero or more dimension nodes |
| Value | Y-axis, bubble size, color depth | One attribute per measure node |

![](/assets/Chart_Bubble.png)

#### Contrast

| Design District | chart | requirement |
| :--- | :--- | :--- |
| Row dimension | Y-axis | Only one dimension node can be placed |
| Value | X-axis | Only two index nodes can be placed |

![](/assets/chart_contrast.png)

#### Word Cloud

The cartesian product is labeled according to multiple rows.

![](/assets/chart_wordcloud.png)

#### Tree Map

| Design District | chart | requirement |
| :--- | :--- | :--- |
| Row dimension | Multiple row dimensions represent multiple layers, which are classified by color | Place one or more dimension nodes |
| Value | The area is the number  | Only one index nodes can be placed|

![](/assets/chart_treemap.jpg)

#### Heat Map

| Design District | chart | requirement |
| :--- | :--- | :--- |
| Row dimension | X-axis | Place one or more dimension nodes |
| Column | classify | Place zero or more dimension nodes |
| Value | | Only one index nodes can be placed |

![](/assets/chart_calender.png)

#### Relation

| Design District | chart | requirement |
| :--- | :--- | :--- |
| Row dimension | center point sets | Place one or two dimension nodes |
| Column | classify | Place one or two dimension nodes |
| Value |  | Only one index nodes can be placed |

![](/assets/chart_relation.png)
