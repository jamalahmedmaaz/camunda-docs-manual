---

title: 'Get Historic Details'
category: 'History'

keywords: 'historic get query list'

---


Query for historic details that fulfill the given parameters. 
The size of the result set can be retrieved by using the [count](ref:#history-get-historic-details-count) method.


Method
------

GET `/history/detail`


Parameters
----------  
  
#### Query Parameters

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>processInstanceId</td>
    <td>Filter by process instance id.</td>
  </tr>
  <tr>
    <td>executionId</td>
    <td>Filter by execution id.</td>
  </tr>
  <tr>
    <td>activityInstanceId</td>
    <td>Filter by activity instance id.</td>
  </tr>
  <tr>
    <td>variableInstanceId</td>
    <td>Filter by variable instance id.</td>
  </tr>
  <tr>
    <td>formFields</td>
    <td>Only include <strong>HistoricFormFields</strong>. Values may be <code>true</code> or <code>false</code>.</td>
  </tr>
  <tr>
    <td>variableUpdates</td>
    <td>Only include <strong>HistoricVariableUpdates</strong>. Values may be <code>true</code> or <code>false</code>.</td>
  </tr>
  <tr>
    <td>excludeTaskDetails</td>
    <td>Excludes all task-related <strong>HistoricDetails</strong>, so only items which have no task id set will be selected. When this parameter is used together with <code>taskId</code>, this call is ignored and task details are <strong>not</strong> excluded. Values may be <code>true</code> or <code>false</code>.</td>
  </tr>
  <tr>
    <td>sortBy</td>
    <td>Sort the results by a given criterion. Valid values are <code>processInstanceId</code>, <code>variableName</code>, <code>variableType</code>, <code>variableRevision</code>, <code>formPropertyId</code> or <code>time</code>.
    Must be used in conjunction with the <code>sortOrder</code> parameter.</td>
  </tr>
  <tr>
    <td>sortOrder</td>
    <td>Sort the results in a given order. Values may be <code>asc</code> for ascending order or <code>desc</code> for descending order.
    Must be used in conjunction with the <code>sortBy</code> parameter.</td>
  </tr>
  <tr>
    <td>firstResult</td>
    <td>Pagination of results. Specifies the index of the first result to return.</td>
  </tr>
  <tr>
    <td>maxResults</td>
    <td>Pagination of results. Specifies the maximum number of results to return. Will return less results if there are no more results left.</td>
  </tr>
</table>


Result
------

A json array of historic detail objects.
Each historic detail object has the following properties:

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>id</td>
    <td>String</td>
    <td>The id of the historic detail.</td>
  </tr>
  <tr>
    <td>processInstanceId</td>
    <td>String</td>
    <td>The id of the process instance the historic detail belongs to.</td>
  </tr>
  <tr>
    <td>activityInstanceId</td>
    <td>String</td>
    <td>The id of the execution the historic detail belongs to.</td>
  </tr>
  <tr>
    <td>taskId</td>
    <td>String</td>
    <td>The id of the task the historic detail belongs to.</td>
  </tr>
  <tr>
    <td>time</td>
    <td>String</td>
    <td>The time when this historic detail occurred has the format <code>yyyy-MM-dd'T'HH:mm:ss</code>.</td>
  </tr>
</table>

Depending on the concrete instance of the historic detail it contains further properties. In case of an <code>HistoricVariableUpdate</code> the following properties are also provided:

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>variableName</td>
    <td>String</td>
    <td>The name of the variable which has been updated.</td>
  </tr>
  <tr>
    <td>variableType</td>
    <td>String</td>
    <td>The type of the variable which has been updated.</td>
  </tr>
  <tr>
    <td>value</td>
    <td>String/Number/Boolean</td>
    <td><p>The variable's value if it is a primitive variable. The variable's serialized value if it is a custom object variable with a text-based serialization format. <code>null</code> for variable types that serialize as byte array (i.e. variable types <code>bytes</code> and <code>serializable</code>).</p>
    <!-- TODO: ref variable docs here -->
    
    <p>
    <b>Deprecated</b>: For variables of type <code>serializable</code>, a json object applying Jackson's POJO
    serialization is returned. Note that this is only returned when the involved classes are accessible to the REST resources.
    </p></td>
  </tr>
  <tr>
    <td>serializationConfig</td>
    <td>Object</td>
    <td>A json object containing additional variable meta-data required to interpret the value. Exact properties depend on the variable type. For all primitive variable types this property is <code>null</code>.
    <!-- TODO: ref variable docs here -->
    </td>
  </tr>
  <tr>
    <td>revision</td>
    <td>number</td>
    <td>The revision of the historic variable update.</td>
  </tr>
  <tr>
    <td>errorMessage</td>
    <td>String</td>
    <td>An error message in case a Java Serialized Object could not be de-serialized.</td>
  </tr>
</table>

In case of an <code>HistoricFormField</code> the following properties are also provided:

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>fieldId</td>
    <td>String</td>
    <td>The id of the form field.</td>
  </tr>
  <tr>
    <td>fieldValue</td>
    <td>String/Number/Boolean/Object</td>
    <td>The submitted value.</td>
  </tr>
</table>

Response codes
--------------  

<table class="table table-striped">
  <tr>
    <th>Code</th>
    <th>Media type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>200</td>
    <td>application/json</td>
    <td>Request successful.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>application/json</td>
    <td>Returned if some of the query parameters are invalid, for example if a <code>sortOrder</code> parameter is supplied, but no <code>sortBy</code>. See the <a href="ref:#overview-introduction">Introduction</a> for the error response format.</td>
  </tr>
</table>


Example
-------

#### Request

GET `/history/detail?processInstanceId=aProcInstId`
  
#### Response

    [
      {
        "id": "12345",
        "processInstanceId": "aProcInstId",
        "activityInstanceId": "anActInstId",
        "executionId": "anExecutionId",
        "time": "2014-02-28T15:00:00",
        "variableName": "myProcessVariable",
        "variableTypeName": "String",
        "value": "aVariableValue",
        "serializationConfig": null,
        "revision": 1,
        "errorMessage": null
      },
      {
        "id": "12345",
        "processInstanceId": "aProcInstId",
        "activityInstanceId": "anActInstId",
        "executionId": "anExecutionId",
        "taskId": "aTaskId",
        "time": "2014-02-28T15:00:00",
        "fieldId": "aFieldId",
        "fieldValue": "aFieldValue"
      }
    ]