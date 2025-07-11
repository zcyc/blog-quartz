---
title: "Java Stream"
---


```java
/**
		  * 单线程写法
		  **/
		 List<Object> dictList = new ArrayList<>();
		 for (AttendanceMachine attendanceMachine : list) {
			 String label = attendanceMachine.getName();
			 String value = attendanceMachine.getId();
			 HashMap<String, String> dict = new HashMap<>();
			 dict.put("label", label);
			 dict.put("value", value);
			 dictList.add(dict);
		 }

		 /**
		  * 并行流写法
		  **/
		 List<HashMap<String, String>> dictList2 = list.parallelStream()
				 .map(attendanceMachine -> {
					 HashMap<String, String> dict = new HashMap<>();
					 String label = attendanceMachine.getName();
					 String value = attendanceMachine.getId();
					 dict.put("label", label);
					 dict.put("value", value);
					 return dict;
				 })
				 .collect(Collectors.toList());
```
