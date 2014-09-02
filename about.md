---
layout: angular
title: "关于：About"
---
####Basic Info
My name is Zhang Chao, living in ShangHai city.  

####IT skills  
1.Github：https://github.com/zhangchao  

####Work Experience  


####Resume
<ul>
  <li ng-repeat="message in messages">
    <input ng-model="message.text" ng-change="messages.$save(message)" />
    <button ng-click="messages.$remove(message)">X</button>
  </li>
</ul>