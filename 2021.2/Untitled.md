```
// if (!tenderEvaluationConfigService.isMarkRate(userIndexMap)) {
//     Map<String, String> userMarkDataExtend = tenderEvaluationConfigService.getUserMarkData(markingData, userIndex.get(0), tableData.userColumnsMap, String.valueOf(pageTreeData.id));
//     tenderEvaluationConfigService.markRateAndNoExtendAmount();
//     if (markingData != null && markingData.containsKey(String.valueOf(pageTreeData.id))) {
//         Map<String, Map<String, String>> userMarkDatas = markingData.get(String.valueOf(pageTreeData.id));
//         Map<String, String> userMarkDataExtend = null;
//         String userIndexId = userIndex.get(0).equals("pte") ? "" : userIndex.get(0);
//         if (tableData.userColumnsMap.containsKey(userIndexId)) {
//             userIndexId = tableData.userColumnsMap.get(userIndexId).userId;
//         }
//         if (userMarkDatas.containsKey(userIndexId)) {
//             userMarkDataExtend = userMarkDatas.get(userIndexId);
//         }
//         // 非扩展列
//         if (userMarkDataExtend != null && userMarkDataExtend.containsKey("notExtendColor")
//             && !userMarkDataExtend.get("notExtendColor").equals("")) {
//             JSONObject greatObj = rData.getJSONObject("-1");
//             greatObj.put(index - 1 + "", userMarkDataExtend.get("notExtendColor"));
//         }
//     }
// }
```



![image-20210220175858100](C:\Users\zhangyq-i\AppData\Roaming\Typora\typora-user-images\image-20210220175858100.png)

```

// for (int mergindex = 0; mergindex < mergIds.size(); mergindex++) {
                //     ColTypeSource mergValue = mergIds.get(mergindex);
                //     if (mergValue.rownum == 0) {
                //         mergValue.isMerged = true;
                //         rows.add(mergValue);
                //         cloNum.set(mergValue.colnum);
                //     } else if (mergValue.rownum == 1 && (mergValue.colnum == mergValue.colnum)) {
                //         isRowMerg.set(false);
                //     }
                // }
```

