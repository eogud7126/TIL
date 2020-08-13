## 뒤로 갔을 때 SearchBar 사라지지 않는 이슈

- 검색을 한 후 검색바를 끄지 않고 뒤로가기 버튼을 눌렀을 때 검색바와 함께 뒤로 가는 이슈를 발견했습니다.

<img src="./searchbar_issue.gif" width="200" height="400" />



- 이 문제를 해결하기 위해 메일 리스트 뷰 컨트롤러의 viewWillDisappear에서 searchController를 사라지도록 구현을 했습니다.

<img src="./searchbar_solve.gif" width="200" height="400" />

