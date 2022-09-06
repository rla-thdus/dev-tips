# mongorestore 사용해서 데이터 복원했는데 데이터베이스의 크기가 달라진 이유
데이터 복원을 하기 위해서 `mongorestore` 을 사용했는데, 그 전에 확인했던 데이터베이스 사이즈와 복원한 데이터베이스 사이즈가 달랐다.<br>
나는 당연히 같은 데이터를 복원한 것이니까 같을 줄 알았는데 너무 많이 차이가 나서 당황스러웠다.

그래서 데이터가 제대로 들어가지 않은건가 싶어서 확인해봤는데 그건 아니였다, 컬렉션도 다 존재했고, 그 안의 데이터들의 개수도 같았다.

## 이유
찾아본 결과 배포할 원래 데이터베이스에 재사용할 수 있는 공간이 많은 경우 복원 시 저장소 크기가 달라질 수 있다고 한다.<br>
재사용할 수 있는 공간이 많은 경우는 문서 삭제로 인해 남는 공간이 생기는 등의 상황이 있다.

그리고 WiredTiger 스토리지 엔진을 사용하는데, 이 엔진을 사용하면 MongoDB의 모든 컬렉션 및 인덱스에 대한 압축을 지원한다고 한다<br>
따라서 압축된 데이터가 들어가져 사이즈가 줄어든 것 같다.

## 정리
- 데이터베이스의 사이즈는 달라질 수 있기 때문에 자세히 비교를 하기 위해서는 `show dbs` 로 나오는 결과가 아니라 **`db.stats()`** 를 사용해야 한다.

---

[^1] https://stackoverflow.com/questions/68886634/mongorestore-4-2-decreases-the-restored-database-sizes-to-half<br>
[^2] https://serverfault.com/questions/993995/mongo-restore-command-reduces-database-size
