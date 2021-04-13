회사에서 개발하고 있는 솔루션에 협업 기능을 넣고자 Canvas 작업을 하고 있습니다.

오픈 소스인 fabric.js 라이브러리를 사용하였고 버전은 4.3.1 버전을 사용하였습니다.

Polygon을 그리기 위해서 구글에 서치를 해보면 AddPoint 함수로 Array에 포인트를 추가한 뒤 0번째 Array와 같은 포인트일 때 Polygon을 Canvas에 추가하는 소스가 많이 나옵니다.

저 역시 해당 소스를 가져다가 쓰려고 했는데 문제가 발생합니다.

Canvas에서 Zoom In/Out이 일어나거나 Panning이 발생할 경우 좌표 정보가 틀어져서 Polygon Point가 엉뚱한 곳에 잡힌다는 것입니다.

마우스가 클릭된 좌표를 가져오기위해선 이벤트의 layerX, layerY 를 사용합니다. 이는 현재 레이어를 기준으로 이벤트의 수평/수직 좌표입니다.

현재 레이어가 기준이므로 만약 줌 인 줌 아웃이 되었을 때 좌표 정보를 잘못 가져오게 되어서 viewPort를 변경해가면서 가져와야합니다.

* ViewPort란?
- 전체 window에서 실제로 화면이 그려질 직사각형의 영역

fabric object로 생성한 canvas의 viwportTransform을 드래깅이 발생할 때, 마우스 휠 이벤트가 발생할 때 조정을 해야하는 것입니다.

그러나 이렇게 하면 소스도 길어지고 이해도 힙듭니다. (무엇보다 귀찮다!)

다행히 fabric.js에서는 getPointer라는 함수를 제공함으로써 이 귀찮은 비용을 줄여줍니다.

* getPointer 소개
getPointer(e, ignoreZoom) → {Object}
Returns pointer coordinates relative to canvas. Can return coordinates with or without viewportTransform. ignoreZoom false gives back coordinates that represent the point clicked on canvas element. ignoreZoom true gives back coordinates after being processed by the viewportTransform ( sort of coordinates of what is displayed on the canvas where you are clicking. ignoreZoom true = HTMLElement coordinates relative to top,left ignoreZoom false, default = fabric space coordinates, the same used for shape position To interact with your shapes top and left you want to use ignoreZoom true most of the time, while ignoreZoom false will give you coordinates compatible with the object.oCoords system. of the time.

대충 캔버스를 기준으로 포인터 좌표를 반환하며, vieportTransform을 사용하지 않고 좌표를 반환해준다는 내용입니다.

적용된 코드는 해당 위치에 있습니다.

https://gist.github.com/InsoooooooooJANG/591d573e83c20319da4efc1576d34081

이제 Zoom, Panning이 일어나도 다각형을 자유롭게 그릴 수 있습니다
