# api规范

## models

### times

```json
{
    "time": {
        "factory" : "$factory | string", //工厂，例如zjut.zf
        "data": [
            {
                "key": {
                    "year": 2020,
                    "term": 1,
                    "exam": 0
                },
                "period": {
                    "begin": "2020-7-13",
                    "end": "2020-9-10",
                    "tableid": 0 //引用的tableid
                },
            }
        ]
    }, 
    "table": {
        "tableid": 0, //时间表的id
        "tabletype": "平时", //时间表类别
        "cells": {
            "weekofday": "0-6", //覆盖的周数
            "cellid": 1, //第几节课
            "start": "" , //开始时间
            "end": "" //结束时间
        }
    }
}
```

### course-table

```json
{
    "time": { 
        "factory": "$factory | string", //工厂，例如zjut.zf
        "key": {
            "year": 2020, //年份
            "term": 1, //学期
            "exam": 0, //表示是否是考试周
        }
    },
    "info": {
        "courseid": "$id", //课程的id
        "classid": "$id", //教学班的id
        "name" : "$name", //课程名称
        "classname": "$name", //教学班名称，例如0001，optional
        "type": "$type", //课程类型，例如必修课等，optional
        "classscore": "$score", //学分
        "classtime": "$time", //学时
        "examtype": "考察", //考核方式
        "period": [ //表示上课的时间
            {
                "periodid": "", //上课时间的id
                "weeks": "1-16|2", //开课周数，格式之后再讲
                "teacherid": "", //教师id号
                "teacher": "", //上课教师
                "campus": "", //上课校区
                "location-id": "", //上课地点
                "location": "", //上课地点
                "weekofday": "5", //表示星期几上课，星期日上课时用0表示
                "timetype": 0, //时间标记类型，0表示依赖time约定的表格时间，1表示使用特殊时间，时间格式满足hh:MM-hh:MM的格式
                "timeofday": "6-7", //表示时间
            }
        ]
    },
    "rewrites": [ //课表修正，暂时不考虑使用
        {
            "classid": "", //要修正的课程id
            "periodid": "", //要修正的时间id
            "week": 9, //要修正的星期
            "weeokofday": "5",
            "timetype": 0,
            "timeofday": "6-7"
        }
    ]
}
```



