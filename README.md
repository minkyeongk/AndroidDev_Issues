# AndroidDev_Issues
안드로이드 개발에서 겪었던 이슈들과 해결 방안


**1. 커스텀뷰 작성 시 constructor가 여러개여야 함**  
```
constructor 앞에 @JvmOverloads 붙임  
@JvmOverload constructor( ~  
```   
참고: https://medium.com/@futureofdev/%EC%BD%94%ED%8B%80%EB%A6%B0-kotlin-customview-774e236ca034
   

**2. uri에서 bitmap 가져오는 코드** 
```
(MediaStore.Images.Media.getBitmap(this.getContentResolver(), data?.data))  
인자: (컨텐츠 리졸버, uri)
```
         
      
**3. 리싸이클러뷰 순서 꼬임 문제 해결: 어댑터에 아래 함수 오버라이드 해주기 (이외에도 필요한 기본 오버라이드 함수 빼먹지 말기)**
```
override fun getItemId(position: Int): Long {  
return items[position].hashCode().toLong()  
} 
```      
      

**4. String 형식으로 받아온 zonedDateTime 변수를 Date 형식으로 변환하기 (사실 이건 맞는 형식 찾아서 파싱하는게 더 어려웠던 것 같은데.. 코드 다시 확인하기)**
```
val zonedDate = ZonedDateTime.parse(writeTime)  
val date = Date.from(zonedDate.toInstant())
```
      
   
**5. Fragment 새로고침 코드: 기존 Fragment를 단순히 detach하고 attach하는 것이 아니라 새로운 프래그먼트 생성해 replace 처리(데이터 로딩 함수가 다시 처음부터 동작하도록 하기 위해)**
```
val newFragment: CommunityFragment = CommunityFragment()  
val ft = this.fragmentManager?.beginTransaction()  
ft?.replace(this.id, newFragment)?.addToBackStack(null)?.commit()  
```
      
     
 **6.startActivityForResult 로 실행된 액티비티를 새로고침 하는 경우, 새로고침한 Activity와 처음 호출한 프래그먼트 간 통신 불가 (OnActivityResult 활용)**   
```
새로고침을 액티비티 finish 후 프래그먼트 OnActivityResult에서 재호출로 구현, 화면전환 애니메이션 제거  
```
