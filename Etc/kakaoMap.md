## kakao Map 사용하기
<br>
카카오 맵을 뿌려줄 때에 필요한 건 일단 kakao dev에 프로젝트를 등록하고 
map api를 사용할 권한이 있는 appKey를 발급 받는 것이다.

이 작업들은 완료 되었다는 전제 하에, 구현하는 방법을 알아보자.

1. 우선 _document.tsx에 script를 불러오는 처리를 해야 한다.

```tsx
{kakaoDevAppKey && (
  <script
    type="text/javascript"
    src={`//dapi.kakao.com/v2/maps/sdk.js?appkey=${kakaoDevAppKey}`}
  />
)}
```

2. 그리고 나서, 지도를 뿌릴 곳에다가 아래 설정을 해주면 된다.

```tsx
const kakaoMap = useRef<HTMLDivElement>(null);
useEffect(() => {
    if (kakaoMap && kakaoMap.current) {
      // 좌표 (적당한 좌표)
      const x = 126.972829;
      const y = 37.559278;

      const coords = new (window as any).kakao.maps.LatLng(y, x); // 지도의 중심좌표

      const options = {
        center: coords,
        level: 2,
      };
      const map = new (window as any).kakao.maps.Map(kakaoMap.current, options);
      const marker = new (window as any).kakao.maps.Marker({
        position: coords,
        map,
      });
      // 맵의 중앙으로 이동
      map.relayout();
      map.setCenter(coords);
      // 마커를 중앙으로 이동
      marker.setPosition(coords);

      kakaoMap.current.style.width = '100%';
      // 브라우저 사이즈를 줄일 경우, 다시 정중앙에 포인트로 오도록 처리하기
      document.body.onresize = function (e) {
        map.setCenter(coords);
        marker.setPosition(coords);
        map.relayout();
      };

      // 아래는 필요 시 주석 해제 후 사용
      // 일반 지도와 스카이뷰로 지도 타입을 전환할 수 있는 지도타입 컨트롤을 생성합니다
      // const mapTypeControl = new (window as any).kakao.maps.MapTypeControl();

      // 지도에 컨트롤을 추가해야 지도위에 표시됩니다
      // kakao.maps.ControlPosition은 컨트롤이 표시될 위치를 정의하는데 TOPRIGHT는 오른쪽 위를 의미합니다
      // map.addControl(
      //   mapTypeControl,
      //   (window as any).kakao.maps.ControlPosition.TOPRIGHT
      // );

      // 지도 확대 축소를 제어할 수 있는  줌 컨트롤을 생성합니다
      // const zoomControl = new (window as any).kakao.maps.ZoomControl();
      // map.addControl(
      //   zoomControl,
      //   (window as any).kakao.maps.ControlPosition.RIGHT
      // );
    }
  }, [kakaoMap]);

return <KakaoMapContainer ref={kakaoMap} />;
```

<br>
참고 사이트

[https://yong-nyong.tistory.com/16](https://yong-nyong.tistory.com/16)