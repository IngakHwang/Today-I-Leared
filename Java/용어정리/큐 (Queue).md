# 큐 (Queue)



## Queue의 특징

- 맨 앞(front)에서 자료를 꺼내거나 삭제하고, 맨 뒤(rear)에서 자료를 추가함
- First In First Out(선입선출) 구조
- 일상 생활에서 일렬로 줄 서 있는 모양
- 순차적으로 입력된 자료를 순서대로 처리하는 데 많이 사용되는 자료구조
- 콜센터에 들어온 문의 전화, 메세지 큐 등의 활용됨
- jdk 클래스 : ArrayList



## LinkList를 활용하여 Queue 구현

MyListQueue.java

```java
import linkedlist.MyListNode;
import linkedlist.MyLinkedList;

interface IQueue{
	public void enQueue(String data);
	public String deQueue();
	public void printAll();
}

public class MyListQueue extends MyLinkedList implements IQueue{

	MyListNode front;
	MyListNode rear;
		
	
	public MyListQueue()
	{
		front = null;
		rear = null;
	}
	
	public void enQueue(String data)
	{
		MyListNode newNode;
		if(isEmpty())  //처음 항목
		{
			newNode = addElement(data);
			front = newNode;
			rear = newNode;
		}
		else
		{
			newNode = addElement(data);
			rear = newNode;
		}
		System.out.println(newNode.getData() + " added");
	}
	
	public String deQueue()
	{
		if(isEmpty()){
			System.out.println("Queue is Empty");
			return null;
		}
		String data = front.getData();
		front = front.next;
		if( front == null ){  // 마지막 항목
			rear = null;
		}
		return data;
	}
	
	public void printAll()
	{
		if(isEmpty()){
			System.out.println("Queue is Empty");
			return;
		}
		MyListNode temp = front;
		while(temp!= null){
			System.out.print(temp.getData() + ",");
			temp = temp.next;
		}
		System.out.println();
	}
}

```

