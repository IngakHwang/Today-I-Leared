# TIL

## Java Hashmap 사용

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Set;

public class MapMain {
	
	public static void main(String[] args) {
		
		MapMain mm = new MapMain();
		mm.test1();
	}
	
	public void test1() {
		//HashMap, HashTable
		HashMap<String, Integer> map = new HashMap<String, Integer>();
		//map 인스턴스 -> Key:Value = String : Interger
        
        // 신규입력
		map.put("one", 1);			// Key:Value = "one":1
		map.put("two", 20);			// Key:Value = "two":20
		map.put("three", 3);		// Key:Value = "three":3
	
        int size = map.size();	
        System.out.println(size);	// 3
        
		int val = map.get("two");
		System.out.println(val);	// 20	
		
        // 수정 왜?why? => 이미 입력되었던 Key이기 때문
		map.put("two", 2);			// 20 -> 2
		val = map.get("two");		// Key:Value = "two":2
		System.out.println(val);	// 2
	
        
        // Key = three인 항목 삭제
		val = map.remove("three");	// 삭제한 값 val 변수에 담기
		System.out.println(val);	// 3
		
        // Set = 순서를 유지하지 않는 데이터 집합
        // 데이터의 중복을 허용X
        
		Set<String> keys = map.keySet();
		
        //Iterator List,Set,Map 정보를 얻기 위함
        //hastNext() => 읽어올 요소가 남아있는지 확인하는 메소드 (boolean)
        //next() => 데이터 반환
        
		Iterator<String> iter = keys.iterator();
		
        
		while(iter.hasNext()) {
			String key = iter.next();
			int value = map.get(key);
			System.out.println(key+ ":"+value );
		}
	
	}
}

```

