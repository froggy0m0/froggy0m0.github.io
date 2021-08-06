---
layout: post
title: java 다익스트라 & 벨만포드 알고리즘구현
categories: Project
message: 컴퓨터 네트워크
order: 1
---

다익스트라 & 벨만포드 알고리즘구현

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-network_path-1.png">

### Dijkstar's Algorithm

| Iteration |       T       | L(1) | Path | L(3) | Path | L(4) | Path | L(5) | Path  | L(6) | Path    |
| :-------: | :-----------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :---: | :--: | ------- |
|     1     |     {2,4}     |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  ∞   |   -   |  ∞   | -       |
|     2     |     {2,4}     |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  3   | 2-4-5 |  ∞   | -       |
|     3     |    {1,2,4}    |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  3   | 2-4-5 |  ∞   | -       |
|     4     |   {1,2,3,4}   |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  3   | 2-4-5 |  8   | 2-3-6   |
|     5     |  {1,2,3,4,5}  |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  3   | 2-4-5 |  5   | 2-4-5-6 |
|     6     | {1,2,3,4,5,6} |  3   | 2-1  |  3   | 2-3  |  2   | 2-4  |  3   | 2-4-5 |  5   | 2-4-5-6 |

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-network_path-2.png">

```java
package Dijkstra;

	
																		
class Dijkstra {														
    //각인덱스는 0부터 시작하지않고 1부터 시작하-기때문에 필요한값의 +1
	private int[][] weight = new int[7][7]; 							
    //2차원 배열 weight에 각 꼭지점의 가중치를 저장
	private static String[] saveRoute = new String[7];					
    //경로를 나타낼 배열
	private static int[] T = new int[7];								
    //T 집합을 나타낼배열
	private static int count = 0;										
    //횟수를 나타낼 count 변수
	static boolean[] visited = new boolean[7];							
    //각 꼭지점의 방문 여부						
	public static int distance[] = new int[7]; 							
    //근원지노드에서 각 노드까지의 거리
	static int INF=999;
	static String SINF ="INF";


public void input(int v1, int v2, int w) {								
    //가중치입력
	weight[v1][v2] = w;
}


public  void arg(int Tnode,int source) {								
	//알고리즘2단계
	
	count += 1;
	T[count] = Tnode;													
    //T집합에 이전 T집합과 최소비용을갖는 다음T를 넣는다
	visited[Tnode] = true;
	
	int mindis = INF;
	int minver = 0;
	
	if (count ==1) {
		for(int i=1; i<=6; i++) {
			distance[i] = INF;
			saveRoute[i] = "-----";
			
			if (i == source) {												
				distance[source] = 0;										
                //근원지의 최소거리값에 0
				saveRoute[source] = source+"";								
                //근원지의 경로값에 근원지를저장
			}
	 	}
	}

	for (int i = 1; i <= 6; i++) {
		if (!visited[i] && weight[Tnode][i]!=0) {						
            //방문하지않았고 Tnode 에서 i 까지 의 거리가 0이아닐경우에
			if (distance[i] > distance[Tnode]+ weight[Tnode][i]) {		
                //이미 계산된결과값과 새로운 경로의 결과값과 비교
				distance[i] = distance[Tnode]+ weight[Tnode][i]; 		
				saveRoute[i] ="";										
                //값이 갱신되면 saveRout 경로를 초기화해주고 다시저장
				saveRoute[i] += saveRoute[Tnode] +"-"+ i;				
			}
		}
		if(!visited[i] && distance[i] < mindis) {						
            //방문하지않았고 새로운 값이 갱신될때마다  그거리와 노드를 저장 
			mindis = distance[i];										
			minver = i;													
		}																
        //다음 계산할 T값을 찾기위해 	
	}
	
	print(source);															
    //과정마다 T , 각노드의 최소거리 경로를 출력

	if (count == 6) {													
        //모든노드를 방문했으면 끝
		return;
	}else {
		arg(minver,source);												
        //노든노드들이 T집합에들갈떄까지 반복
	}
	

}

	public static void print(int source) {
		
		if(count == 1) {												
		System.out.println("┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐");
		System.out.println("│Iteration │  T   │     L(1)     │     PATH     │     L(3)     │     PATH     │     L(4)     │     PATH     │     L(5)     │     PATH     │     L(6)     │     PATH     │");
		System.out.println("└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘");
		}
		
		System.out.printf("│%10d|",count);								
        //Iteration 을 나타냄

		
		for (int j = 1; j <= 6; j++) {
			if (visited[j]==true) {										
                //방문한 노드의 출력해서 t집합을 출력
				System.out.printf("%d",j);			
			}else {
				System.out.print(" ");
			}
		}
		
		for (int i = 1; i < T.length; i++) {
			if (i != source) {
                //근원지를 제외한 값들     L(2)와 경로 값을 제외한
				if (distance[i] == INF) {
					System.out.printf("│%14s",SINF);
                    //INF면 문자열 INF 출력
				}else {
				System.out.printf("│%14d",distance[i]);
                    //최소경로와
				}
				System.out.printf("│%14s",saveRoute[i]);
                //최소경로
		}
	}
			
		System.out.print("│");
		System.out.println("");
	}
}
public class Dijkstar {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		Dijkstra a = new Dijkstra();
		
		a.input(1,2,2);//(노드.노드.가중치)
		a.input(1,3,5);
		a.input(1,4,1);
		a.input(2,1,3);
		a.input(2,3,3);
		a.input(2,4,2);
		a.input(3,1,8);
		a.input(3,2,6);
		a.input(3,4,3);
		a.input(3,5,1);
		a.input(3,6,5);
		a.input(4,1,7);
		a.input(4,2,2);
		a.input(4,3,3);
		a.input(4,5,1);
		a.input(5,3,1);
		a.input(5,4,1);
		a.input(5,6,2);
		a.input(6,3,8);
		a.input(6,5,4);


		a.arg(2,2);														
		//arg_1함수로 시작값을 보내줌
		

		
	}






}

```



### Bellmanford's Algorithm

| h    | Lh(1) | Path | Lh(3) | Path | Lh(4) | Path | Lh(5) | Path  | Lh(6) | Path    |
| ---- | ----- | ---- | ----- | ---- | ----- | ---- | ----- | ----- | ----- | ------- |
| 0    | ∞     | -    | ∞     | -    | ∞     | -    | ∞     | -     | ∞     | -       |
| 1    | 3     | 2-1  | 3     | 2-3  | 2     | 2-4  | ∞     | -     | ∞     | -       |
| 2    | 3     | 2-1  | 3     | 2-3  | 2     | 2-4  | 3     | 2-4-5 | 8     | 2-3-6   |
| 3    | 3     | 2-1  | 3     | 2-3  | 2     | 2-4  | 3     | 2-4-5 | 5     | 2-4-5-6 |
| 4    | 3     | 2-1  | 3     | 2-3  | 2     | 2-4  | 3     | 2-4-5 | 5     | 2-4-5-6 |

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-network_path-3.png">

```java
package bellman;

class bellman{
//0은제외했기떄문에 필요한값의+1
	private int[][] weight = new int[7][7];
    //2차원 배열 weight에 각 꼭지점의 가중치를 저장
	private static String[] saveRoute = new String[7];
    //경로를 나타낼배열
	private String[]prevRoute = new String[7];
	private static int hope = 0;
    //hope값을 나타낼 변수
	private static int distance[][] = new int[7][7];
    //근원지에서 각노드 까지의 거리 [홉수][노드]
	private int checkdis[][] = new int[2][7];
    //이전홉값과 현재홉값으 거리를 비교하기위한 체크배열
	static int INF=999;
	static String SINF="INF";


	public void input(int v1,int v2,int w) {
        //가중치 입력
		weight[v1][v2] = w;
	}
	
	public void init(int source) {
		for (int i = 0; i < distance.length; i++) {
            // Hope = 0 일때 초기화과정
			for (int j = 0; j < distance.length; j++) {
				if (j == source) {
					distance[i][j] = 0;
                    //근원지의 거리값은 0으로초기화
					saveRoute[j] = source +"";
                    //근원지의 경로에 근원지추가

				} else {
					distance[i][j] = INF;
                    //그외값모두 INF
					saveRoute[j] = "----";											
				}
			}
		}
		print(source);
        //hope0의 결과값 출력
		arg(source);																
	}
	
	public void arg(int source) {
				
		hope++;
        //arg 함수들어올때마다 홉값 증가
																					
		for (int i = 1; i < distance.length; i++) {
            //checkdis에 이전hope값의 최소거리값을 저장
			checkdis[1][i] = distance[hope-1][i];
			prevRoute[i] = saveRoute[i];
            //prevRoute에 현재저장되있는 경로값을 저장
		}
			for (int i = 1; i < distance.length; i++) {
				for (int j = 1; j < distance.length; j++) {
					if (weight[j][i]!=0) {
                        //j에서i의 가중치가 0이아닌것중에
					if (checkdis[1][i] >= distance[hope-1][j]+weight[j][i]) {
                        //이전값(checkdis배열)과비교해서 계산한값이 이전값이더크면
						distance[hope][i] = distance[hope-1][j]+weight[j][i];
                        //현재홉에 최소값을 갱신
						saveRoute[i] = prevRoute[j] + "-" +i;
                        //saveRoute에 갱신된 이전경로에 + i까지노드를 
					}
					}
				}
				
			}
			
			String check1="";															
			String check2="";
			
			for (int i = 0; i < distance.length; i++) {								
				check1 += distance[hope-1][i];
                //이전홉값저장
				check2 += distance[hope][i];
                //현재홉값저장
			}
			
			if (check1.equals(check2)) {
                //현재홉의거리값들과 이전홉값의거리값들을 비교
				print(source);
				return;						
			}else {
				print(source);
				arg(source);	
			}
			

	}
	public static void print(int source) {
		if(hope == 0) {
			System.out.println("┌─────┬──────────┬──────────────┬──────────┬──────────────┬──────────┬──────────────┬──────────┬──────────────┬──────────┬──────────────┐");
			System.out.println("│  h  │   L(1)   │     path     │   L(3)   │     path     │   L(4)   │     path     │   L(5)   │     path     │   L(6)   │     path     │");
			System.out.println("└─────┴──────────┴──────────────┴──────────┴──────────────┴──────────┴──────────────┴──────────┴──────────────┴──────────┴──────────────┴");
		}
		
		System.out.print("│");
		System.out.printf("%5d│",hope);
		for (int i = 1; i < distance.length; i++) {
			if(i != source) {
				
				if(distance[hope][i]==INF) {
					System.out.printf("%10s│",SINF);
				}else {
			System.out.printf("%10d│",distance[hope][i]);
				}
				
				
			System.out.printf("%14s│",saveRoute[i]);
			}
		}
		System.out.println();
		
	}
}



public class bellmanford {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		bellman a = new bellman();
		
		a.input(1,2,2);
		a.input(1,3,5);
		a.input(1,4,1);
		a.input(2,1,3);
		a.input(2,3,3);
		a.input(2,4,2);
		a.input(3,1,8);
		a.input(3,2,6);
		a.input(3,4,3);
		a.input(3,5,1);
		a.input(3,6,5);
		a.input(4,1,7);
		a.input(4,2,2);
		a.input(4,3,3);
		a.input(4,5,1);
		a.input(5,3,1);
		a.input(5,4,1);
		a.input(5,6,2);
		a.input(6,3,8);
		a.input(6,5,4);


		a.init(2);
	}

}
```

