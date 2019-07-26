### 1、billAttendanceCheck
考勤结果审核，是否和输入一致
### 2、getNextPerson
设置审核人
```
var dataSubmit = {};
dataSubmit.bosType = "BCC47DEA";    //bosType为分录号

mbos('nextperson1').checkParticipantPerson({    //nextperson1为提交审核人控件id
    editdata: dataSubmit,
    callback: _fn       //_fn为设置审核人的回调函数。
});
```
### 3、后台组装实体元数据字段
### 4、提交新数据，字段需与后台一致。