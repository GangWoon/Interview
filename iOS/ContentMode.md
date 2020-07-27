# ContentMode

---

UIView에 ContentMode라는 속성이 있다. 

컨텐츠 사이즈를 어떻게 조절할지 어디에 배치할지 결정하는 정책이다. 

view는 자신이 보여줄 content를 비트맵 데이터를 캐싱하고 있다. (view bound가 변하거나 content를 그릴 때)



### ScaleToFill (default)

content를 view의 크기에 딱 맞게 크기를 조절한다. content의 비율이 유지되지 않아서 찌르러져 보일 수 있다.



### AspectFit(ScaleAspectFit)

content를 원래 비율로 유지하면서 뷰의 크기에 맞춘다. content가 잘리지 않을 정도로만 크기를 조절한다.



### AspectFill(ScaleAspectFill)

AspectFit처럼 원래 비율을 유지하지만, view의 크기에 맞춰서 조절한다. content의 일부가 잘릴 수 도 있다.



### Redraw

캐시된 비트맵을 사용하지 않고, 매번 컨텐츠를 새로 그린다. 이때 setNeedDisplay가 호출된다.



나머지 옵션들은 컨텐츠 크기를 조절하지 않고, 컨텐츠가 배치되는 위치에만 영향을 준다.











