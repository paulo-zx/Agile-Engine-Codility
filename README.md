# Agile-Engine-Codility

1 QUESTÃO

Write a function:

class Solution { public int solution(int[] A); }

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.

Given A = [−1, −3], the function should return 1.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..100,000];
each element of array A is an integer within the range [−1,000,000..1,000,000].
Copyright 2009–2023 by Codility Limited. All Rights Reserved. Unauthorized copying, publication or disclosure prohibited.


SOLUÇÃO

import java.util.*;

class Solution {
		    public int solution(int[] arr) {

		        Arrays.sort(arr);

		        int smallest = 1;

		        for (int i = 0; i < arr.length; i++) {
		            if (arr[i] == smallest) {
		                smallest++;
		            }
		        }

		        return smallest;
		    }
		}
    
   
 2 QUESTÂO
 
 The system you work with on a daily basis runs multiple microservices. You have been asked to prepare an
aggregation that represents number of user visits to all microservices, for use by data analysis.


The architect you are working with has already designed the API, which consists of a single class:VisitCounter.
VisitCounter has a single method,count.


It returns Map<Long , Long> - this map should contain the number of visits by the user with a given ID.


This method accepts an array of Map<String , UserStats>. Every Map represents the total number of visits per
user to a given microservice. There are some problems, however:


1. The Map key, which is a string, should be parseable to Long - but it may not be. You must skip any such
faulty entries.(())


2. for some keys, UserStats may be null. You must skip any such faulty entries.(())


3. UserStats has a single field, visitCount, of type Optional<Long>. A getter for this field is also
implemented. This field will never be null; however it might be empty. You must skip any such faulty
entries.(())


Remember that you may receive some invalid input: null,empty maps, and so on. Handle it all appropriately
and return an empty map/
You may use Java 8 Streams API to implement your solution.
  
ELES OFERECEM ESTE CODIGO INICIAL
  


import java.util.*;


public class Test {

    public static void main(String[] args) {
        
        class VisitCounter {
        
            Map<Long, Long> count(Map<String, UserStats >... visits) {
                return null;
            }
        }
    }
}
  
  MINHA RESOLUÇÃO
  
import java.util.*;
  
import java.util.stream.*;

class VisitCounter {

    Map<Long, Long> count(Map<String, UserStats>... visits) {
        Map<Long,Long> resultCount = new HashMap<Long,Long>();
        if(visits != null){
       
            resultCount =  Arrays.stream(visits)
            .filter(Objects::nonNull)
            .flatMap(map -> getUserCountMap(map).entrySet().stream())
            .collect(Collectors.toMap(Map.Entry::getKey,
                                      Map.Entry::getValue,
                                      (v1,v2)->v1+v2));
  
    }
        return resultCount;
    }
  
    
   public Map<Long,Long>  getUserCountMap(Map<String, UserStats> map) {
  
       Map<Long,Long> resultCount = new HashMap<Long,Long>();
       
       map.forEach((k,v)->{
           try {
                
               String key = (String) k;
               UserStats userValue = (UserStats) v;
               Long userId = new Long(key);
               Optional<Long> count = userValue.getVisitCount();
               count.ifPresent(aLong -> resultCount.put(userId, (resultCount.getOrDefault(userId, 0l) + aLong)));

           } catch (Exception e) {
           } 
       }
       );
           return resultCount;
       
   }
}
  
  
