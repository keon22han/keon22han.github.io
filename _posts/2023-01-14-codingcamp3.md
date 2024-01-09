---
layout: single
title: "코딩캠프 미니프로젝트 마무리"
categories: coding
tag: [java, game]
toc: true
---


2023.01.09 ~ 2023.01.12 코딩캠프

- 탱크 게임

---

# 탱크 게임

- CannonMain.java
- CannonFrame.java
- CannonPanel.java
- GamePanel.java
- CannonThread.java
- TargetThread.java

총 6개의 java 파일로 이루어진 미니 프로젝트

---

- `CannonMain.java`  : CannonFrame 객체를 생성하여 게임을 실행시켜주는 Main 메소드가 작성되어있는 파일
- `CannonFrame.java`  : CannonFrame Class 를 구현해놓은 java 파일. CannonPanel을 ContentPane으로 설정한다
- `CannonPanel.java`  : 게임 진행 시의 포탄수를 조정하고 , 발사 버튼을 누르면 CannonThread가 실행되어 게임 진행이 가능하게 해주는 Panel
- `GamePanel.java`  : 게임이 실제로 진행되는 패널.  CannonThread 스레드가 생성되고 실행되면 GamePanel에서 제공하는 메소드를 통해 GamePanel에 있는 여러 객체들을 제어 ,  KeyAdapter 이벤트리스너를 통해 키로도 객체들을 제어하며 게임이 진행된다.
- `CannonThread.java`  : 스레드의 메소드에는 총알이 움직이는 모습을 보여주는 moveBullet() 메소드가 있고 , wait-notify 기능과 Interrupt 를 할 수 있도록 해주는 메소드 ,  그리고 스레드가 실행될 수 있도록 해주는 run() 메소드가 있다.
- `TargetThread.java`  : 게임에서 움직이는 Target의 움직임을 구현하는 스레드.

```java
public class CannonMain {
	public static void main(String[] args) {
		new CannonFrame();  //게임 실행
	}
}
```

```java
import java.awt.Color;
import javax.swing.JFrame;

public class CannonFrame extends JFrame{
	private CannonPanel cp=new CannonPanel(); //ContentPane으로 설정할 CannonPanel 선언
	public CannonFrame() {
		super("Tank Game"); //Frame 의 제목
		setSize(800,500); //Frame의 크기를 정해준다
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 게임 Frame을 닫을 시 mainThread도 종료된다
		setLocation(300,200); //게임 실행 시 Frame의 위치 설정
		setResizable(false); //크기를 조정 불가능하게 한다
		setContentPane(cp); // CannonPanel을 ContentPane으로 설정
		setVisible(true); // 현재 Frame과 부착된 컴포넌트들이 보이게
	}
}
```

```java
import java.awt.Color;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ComponentAdapter;
import java.awt.event.ComponentEvent;
import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextField;

public class CannonPanel extends JPanel{
	private int width; //Panel의 width , 바로 알기 위해서는 ComponentListener 이벤트리스너를 통해 얻어와야한다
	private int height; //Panel의 height , 바로 알기 위해서는 ComponentListener 이벤트리스너를 통해 얻어와야한다
	private int count; //대포알의 갯수
	private GamePanel gamePanel=new GamePanel(this,100,300); //게임패널을 이 패널의 중앙에 붙이기
	
	private JLabel countLabel=new JLabel("포탄수");
	private JTextField textField1=new JTextField(); //텍스트필드
	private JButton shootButton=new JButton("발사"); //발사 버튼
	public CannonPanel() {
		setLayout(null); //배치관리자 null로 설정
		setBackground(new Color(146,146,146));
		//버튼, 텍스트 필드 등 배치.
		JButton startbtn=new JButton("발사"); //포 발사 버튼 , 누를 때마다 대포알 벡터에 추가하며 스레드 실행.
		this.addComponentListener(new ComponentAdapter() {
			@Override
			public void componentResized(ComponentEvent e) {
				width=getWidth();
				height=getHeight();
				
				textField1.setBackground(new Color(180,180,180));
				textField1.setSize(50,20);
				textField1.setLocation(width/2-25,height-80);
				countLabel.setSize(50,20);
				countLabel.setLocation(width/2-70,height-80);
				shootButton.setSize(80,40);
				shootButton.setLocation(width/2-55,height-50);
		
				
				shootButton.addActionListener(new ActionListener() {
					@Override
					public void actionPerformed(ActionEvent e) {
						
						gamePanel.setBeforeCount();
						count=Integer.parseInt(textField1.getText());
						gamePanel.setScore(0);
						gamePanel.setScoreLabel();
						//인자로 갯수 넘겨주어야 함 (포탄 수 , 사거리 , 각도)
						gamePanel.startCannonThread(count); //버튼 누르면 bulletThread 실행
						gamePanel.requestFocus();
					}
				});
				add(textField1);
				add(countLabel);
				add(shootButton);
			}
		});
		gamePanel.setSize(750,350);
		gamePanel.setLocation(18,15);
		this.add(gamePanel);
	}
}
```

```java
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Point;
import java.awt.event.ComponentAdapter;
import java.awt.event.ComponentEvent;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.util.Vector;

import javax.swing.JLabel;
import javax.swing.JPanel;

public class GamePanel extends JPanel{ //실제 게임이 진행되는 패널
	private int width;
	private int height;
	private int x; //포탄의 시작 x좌표
	private int y; //포탄의 시작 y좌표
	private int score=0; //포탄 맞은 갯수
	private Tank tank=new Tank(100,100); //탱크의 좌표를 주어주며 객체 생성
	private Vector<Bullet> bulletVector=new Vector<Bullet>(); //대포알 수만큼 벡터에 저장
	private Target target; //타겟의 시작 , 끝 지점 정보를 가지고있어야함
	private CannonThread cannonThread=null;
	private int bulletSize; //대포알 수 지정되는 int 형 필드
	private CannonPanel cannonPanel; // CannonPanel에서 GamePanel 메소드를 사용
	private JLabel scoreLabel=new JLabel("0");
	private JLabel beforeScoreLabel=new JLabel("0");
	private JLabel nowLabel=new JLabel("이번 게임");
	private JLabel currentLabel=new JLabel("이전 게임");
	private TargetThread targetThread; //타겟 스레드 레퍼런스 선어
	
	public GamePanel(CannonPanel cannonPanel,int x,int y) {
		this.setLayout(null);
		this.cannonPanel=cannonPanel;
		this.x=x;
		this.y=y;
		this.tank=new Tank(x,y); //위치 조정은 인자로 받아오게 하자
		setBackground(Color.WHITE);
		//this.bullet=new Bullet(x+10,y,10); //얘도 위치조정은 인자로
		this.addComponentListener(new ComponentAdapter() {
			@Override
			public void componentResized(ComponentEvent e) {
				width=getWidth();
				height=getHeight();
				
				GamePanel.this.setFocusable(true);
				scoreLabel.setFont(new Font("고딕",Font.ITALIC,40));
				scoreLabel.setSize(100,50);
				scoreLabel.setLocation(width/2-25,40);
				beforeScoreLabel.setFont(new Font("고딕",Font.ITALIC,40));
				beforeScoreLabel.setSize(100,50);
				beforeScoreLabel.setLocation(width/2-25,0);
				nowLabel.setFont(new Font("고딕",Font.BOLD,30));
				nowLabel.setSize(200,50);
				nowLabel.setLocation(width/2-180,40);
				currentLabel.setFont(new Font("고딕",Font.BOLD,30));
				currentLabel.setSize(200,50);
				currentLabel.setLocation(width/2-180,0);
			}
		});
		this.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e){
				int keyCode=e.getKeyCode();
				if(keyCode==40 && tank.getY()<310) { //arrow down
					tank.setY(tank.getY()+10);
				}
				else if(keyCode==38 && tank.getY()>10) { //arrow up
					tank.setY(tank.getY()-10);
				}
			}
		});
		
		GamePanel.this.add(scoreLabel);
		GamePanel.this.add(beforeScoreLabel);
		GamePanel.this.add(nowLabel);
		GamePanel.this.add(currentLabel);
	}
	
	public void setBulletSize(int bulletSize) { //대포알 수 지정 메소드
		this.bulletSize=bulletSize;
	}
	public void startCannonThread(int count) { //인자 : 대포알 갯수
		bulletVector=new Vector<Bullet>(); //벡터 객체를 새로 주지 않으면 벡터 객체에 
		//저장되어있는 Bullet 객체들이 스레드가 진행중인 채로 새로운 스레드에서 또 접근을 한다. 물론 전의 스레드를 아래의 cannonThread.setInterrupt()로 삭제할수있지만 벡터 초기화는 기본으로 가져가기
		//그러면 스레드가 여러번 실행되며 이동속도가 훨씬 빨라보이게 된다. 조심할것
		
		score=0; //스코어 초기화된 상태로 스레드 시작
		if(cannonThread!=null) cannonThread.setInterrupt(); //버튼을 눌렀을 때 cannonThread가 존재한다면 (전 판을 했었다면) cannonThread에 interrupt를 해준다
		int height1=(int)(Math.random()*(height-100))/10;
		int height2=height1*10;
		target=new Target(new Point(width/2+200,height2),new Point(width/2+200,height/2)); // 새로운 타겟 객체를 생성한다.
		
		targetThread=new TargetThread(target,this);
		targetThread.start();
		cannonThread=new CannonThread(this,bulletVector,target,count,x,y); //인자로 Vector넘겨주며 스레드 생성
		cannonThread.start();
	}
	public int getScore() { // 현재 점수 반환
		return score;
	}
	public void setScore(int score) { // 현재 점수 세팅
		this.score=score;
	}
	public void setScoreLabel() { // 점수 Label을 설정한다
		scoreLabel.setText(Integer.toString(score));
	}
	public void setScoreZero() { // 현재 점수를 0점으로 초기화
		scoreLabel.setText(Integer.toString(0));
	}
	public void setBeforeCount() { //게임이 끝나면 이전 게임 점수로 반영시켜주는 메소드
		beforeScoreLabel.setText(Integer.toString(score));
	}
	public Tank getTank() { //Tank 객체의 레퍼런스를 반환해주는 메소드
		return tank;
	}
	@Override
	public void paintComponent(Graphics g) { //이 패널에서 게임이 그려진다.
		super.paintComponent(g);
		g.setColor(new Color(64,151,93));
		
		g.fillRect(tank.getX()-10,tank.getY(),60,20); //탱크 그리기
		g.fillRect(tank.getX()+10,tank.getY()-10,20,10); //탱크 그리기
		g.setColor(Color.BLACK);
		g.fillOval(tank.getX()-13,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()-4,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()+5,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()+14,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()+23,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()+32,tank.getY()+15,10,10); //탱크 그리기
		g.fillOval(tank.getX()+41,tank.getY()+15,10,10); //탱크 그리기
		//bullet 그리기
		
		g.setColor(new Color(90,90,90));
		for(int i=0;i<bulletVector.size();i++) {
			Bullet b=bulletVector.get(i);
			g.fillOval(b.getX(),b.getY(),b.getSize(),b.getSize());
		}
		//g.fillOval(bullet.getX(),bullet.getY(),bullet.getSize(),bullet.getSize());
		
		g.setColor(Color.BLACK);
		if(target!=null) g.fillRect(target.getStartPoint().x,target.getStartPoint().y,20,100);
		
	}
}

class Tank{
	private int x; //탱크의 x좌표
	private int y; //탱크의 y좌표
	public Tank(int x,int y) {
		this.x=x;
		this.y=y;
	} //탱크의 x y좌표 초기화
	public int getX() { //x 좌표 반환
		return x;
	}
	public int getY() { //y 좌표 반환
		return y;
	}
	public void setX(int x) { //x 좌표 변경
		this.x=x;
	}
	public void setY(int y) { //y 좌표 변경
		this.y=y;
	}
}
class Bullet{
	private int x; //포의 x좌표
	private int y; //포의 y좌표
	private int size; //포의 size
	
	public Bullet(int x,int y,int size) { //포의 x , y 좌표 . size 초기화
		this.x=x;
		this.y=y;
		this.size=size;
	}
	public int getX() { //x 좌표 반환
		return x;
	}
	public int getY() { //y 좌표 반환
		return y;
	}
	public void setX(int x) { //x 좌표 변경
		this.x=x;
	}
	public void setY(int y) { //y 좌표 변경
		this.y=y;
	}
	public int getSize() { //대포알의 사이즈를 반환
		return size;
	}
}
class Target {
	private Point p1; //target 시작 지점
	private Point p2; //target 끝 지점
	public Target(Point p1,Point p2) {
		this.p1=p1;
		this.p2=p2;
	}
	public Point getStartPoint() { //Target의 시작 포인트 지점
		return p1;
	}
	public Point getEndPoint() { //Target의 끝 포인트 지점
		return p2;
	}
	public void setPoint(Point p) { //Target의 포인트를 바꿔주는 메소드
		this.p1=p;
	}
}
```

```java
import java.util.Vector;

public class CannonThread extends Thread{
	
	private boolean flag=false; // if flag==true -> 스레드 재움
	private GamePanel gamePanel;
	private Target target;
	private Vector<Bullet> bulletVector; //벡터를 넘겨 받고 , 스레드 진행
	private int x; //포탄의 시작 x좌표
	private int y; //포탄의 시작 y좌표
	private int count; //대포알 갯수
	private int count_copy; //대포알 갯수 복사
	private int count1=0; //스레드를 하나 더 사용하지 않고 한 스레드로 대포알 하나가 생성되는
	//시간을 조정하도록 해봄
	private boolean count_up=true; //false 가 되면 더이상 대포알 객체 벡터에 저장하지 않음
	public CannonThread(GamePanel gamePanel,Vector<Bullet> bulletVector,Target target,int count,int x,int y) { //인자로 포의 각도 , 포의 갯수 , 포의 사거리 등 설정
		this.gamePanel=gamePanel;
		this.bulletVector=bulletVector;
		this.count=count;
		this.count_copy=count;
		this.target=target;
		this.x=x;
		this.y=y;
	}
	
	private void moveBullet() {
		if(count_up==true) {
			count1++; //10이 되면 대포알 하나 더 발사
		}
		if(count1%10==0) {
			count_copy-=1; //남은 발사 가능한 볼렛의 수가 줄어들고
			count1=0;
			if(count_copy<0) { //남은 발사 가능한 대포알의 수가 0보다 작다면
				count1=0; //count1은 0으로 바꾸어주지 않아도 딱히 상관은 없음
				count_up=false; //count_up을 false로 바꾸어 더이상 bulletVector에 Bullet객체를 생성하지 않도록 한다
				
			}
			else {
				int y=gamePanel.getTank().getY();
				bulletVector.add(new Bullet(x,y,10));
			}
			
		}
		for(int i=0;i<bulletVector.size();i++) {
			Bullet b=bulletVector.get(i);
			b.setX(b.getX()+10); //이 부분을 좀 바꿔줘야한다. 곡선으로 날아가게끔
			//b.setY(); 이부분도 같이 바꾸어 주어야한다. x , y 좌표를 Math클래스의 메소드를 사용하면 괜찮을 것으로 예상
		}
		
		for(int i=0;i<bulletVector.size();i++) {
			Bullet b=bulletVector.get(i);
			if((b.getX()+10>=target.getStartPoint().x && b.getX()<target.getStartPoint().x+10) && (b.getY()>target.getStartPoint().y && b.getY()<target.getStartPoint().y+100)) {
				bulletVector.remove(i);
				gamePanel.setScore(gamePanel.getScore()+1); //점수 증가
				System.out.print(gamePanel.getScore());
				gamePanel.setScoreLabel();
			}
		}
		if(gamePanel.getScore()==count) {
			gamePanel.setBeforeCount();
			gamePanel.setScoreZero(); //점수 초기화
		}
		gamePanel.repaint();
		gamePanel.revalidate();

	}
	public void setInterrupt() {
		this.interrupt();
	}
	synchronized public void notifyThread() { //다른 클래스에서 스레드 깨우는데 사용
		this.notify();
	}
	public void threadWait() {
		flag=true; //스레드 재우기
	}
	synchronized private void waitThread() {
		try {
			this.wait();
		}catch(Exception e) {
			System.out.print("wait하는 중 오류 발생");
		}
	}
	@Override
	public void run() {
		while(true) {
			try {
				if(flag==true) { //flag가 true라면
					waitThread(); //스레드 재우기
				}
				moveBullet(); //bullet 이동 메소드 호출
				sleep(10);
			}catch(InterruptedException e) {
				System.out.println("interrupt 발생");
				break;
			}
		}
	}
}
```

```java
import java.awt.Point;

public class TargetThread extends Thread{
	Target target;
	GamePanel gamePanel;
	int direction=1; //방향
	public TargetThread(Target target,GamePanel gamePanel) {
		this.target=target;
		this.gamePanel=gamePanel;
	}
	
	private void targetMove() {
		if(target.getStartPoint().y==10) {
			direction=1;
		}
		else if(target.getStartPoint().y==250){
			direction=-1;
		}
		Point p=target.getStartPoint();
		
		target.setPoint(new Point(p.x,p.y+(direction*10)));
		gamePanel.repaint();
		gamePanel.revalidate();
	}
	
	
	@Override
	public void run() {
		while(true) {
			try {
				targetMove();
				sleep(100);
			}catch(Exception e) {
				System.out.println("target 스레드 Interrupt 발생");
			}
		}
	}
}
```

---

### 게임 실행 화면


![](https://velog.velcdn.com/images/keonheehan/post/c6f98bf7-b82f-4b9e-926f-359e2064cc1c/image.png)


게임 실행 시 모습

![](https://velog.velcdn.com/images/keonheehan/post/30a5f6b3-901b-4564-83ce-0a2abb49c6e1/image.png)


![](https://velog.velcdn.com/images/keonheehan/post/54ca46de-b620-4147-8fa3-e55260b26bd1/image.png)


게임 진행중 모습

![](https://velog.velcdn.com/images/keonheehan/post/888764d1-e749-46f2-b8e5-626b1c1bcdf7/image.png)


게임이 끝났을 때의 모습

![](https://velog.velcdn.com/images/keonheehan/post/ead0a96f-5601-441d-b62c-4f4d34b02494/image.png)


게임 재시작 시 전판 점수는 이전게임 점수로 보이는 모습

이 프로젝트를 진행하면서 처음에는 무난하게 진행이 되었는데 , 스레드를 하나로(일정 시간마다 벡터에 Bullet 객체를 저장하는 스레드 , 저장되어있는 Bullet 객체를 제어하는 스레드 두개의 스레드로 코드를 짜는게 훨씬 관리도 편했을 텐데) 짜는 순간부터 조금씩 코드가 꼬이기 시작했다. 다음에 유사한 프로젝트 진행 시에는 스레드를 나누어서 진행해야할 것 같다.

---

## 테트리스 실행결과

그리고 전에 올렸던 게시글에서 테트리스 코드를 완성시키고 올리려했는데 , 코드가 너무 길어져 실행 화면 위주로 올려보려 한다.

![](https://velog.velcdn.com/images/keonheehan/post/804505f7-6dde-4179-bb74-0c10d1a56f75/image.png)


좌측에는 게임이 진행되고 , 우측 상단에는 일시정지버튼과 게임 재개버튼이 보인다. 우측 하단에는 다음에 내려올 블록의 정보를 확인할 수 있다.

![](https://velog.velcdn.com/images/keonheehan/post/1cc14be1-75ff-4259-83c2-bd1267445697/image.png)


버튼을 눌러 일시정지 했을때의 모습이다. 게임 재개시 , 게임 재개버튼에 Focus가 이동하므로 gamePanel.requestFocus();를 해주어야 다시 키가 먹는다.

![](https://velog.velcdn.com/images/keonheehan/post/55deecf9-5e52-4842-a159-2570c017a3ec/image.png)


다시 게임을 재개했을 때 모습이다.

![](https://velog.velcdn.com/images/keonheehan/post/eee86932-f7e1-421a-ac87-2e446dc4bb0d/image.png)


블록이 천장에 닿기 전

![](https://velog.velcdn.com/images/keonheehan/post/f9f9d9c3-f84d-4a18-8bdb-d87724cd5e12/image.png)


천장에 닿게되면 Game Over문구가 나오며 게임이 종료된다. 다시시작 버튼을 눌러 재시작 할 수 있다.

이상으로 코딩캠프에서의 3박 4일동안 제작한 미니 프로젝트였습니다. 감사합니다.