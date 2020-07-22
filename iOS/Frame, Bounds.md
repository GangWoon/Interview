# Frame, Bounds 

CGRect (x:, y:,width:, height: )  -> CGPoint(x: ,y: ) + CGSize(width: ,height: )

* CGPoint 
  * Frame

    상위뷰에 대한 자기 자신의 좌표값.

  

  * Bounds

    자신만의 좌표시스템안에서 나타낸다. (상위 뷰와 상관없다.)

    상위 뷰의 bounds를 (x:50, y:50)을 이동시키면  하위 뷰가 우측아래로 움직이는 게 아닌 좌측 상단으로 올라가는 것 처럼 보이는 이유는 상위 뷰는 50, 50 만큼 움직였고, 그에 따라 하위 뷰는 제자리를 유지하고 있어서 다음과 같은 결과가 나온 것이다.



* CGSize

  * Frame

    뷰의 CGSize를 변경하는 건 스토리 보드에서 뷰의 크기를 변경하는 것 과 동일하다.

    

  * Bounds

    origin으로 부터 줄어든 크기만 큼 계산해서 그려준다.