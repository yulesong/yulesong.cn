---
title: Java获取上周某一天日期1，2，3，4，5，6，7
date: 2019-06-27 15:12:52
categories: 
- Java获取上周某一天日期1，2，3，4，5，6，7
tags: 
- Java
---

## Java获取上周某一天日期1，2，3，4，5，6，7

<!-- more -->

``` bash
public static String getLastWeekDay(int weekDay){
        Calendar calendar = Calendar.getInstance();
        int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK) - 1;
        if(dayOfWeek == 0){
            dayOfWeek = 7;
        }
        int offset = weekDay - dayOfWeek;
        calendar.add(Calendar.DATE,offset-7);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        System.out.println(sdf.format(calendar.getTime()));

}

```
