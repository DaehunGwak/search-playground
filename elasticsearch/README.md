# Elasticsearch 테스트 환경

이 Docker Compose 파일은 Elasticsearch, Kibana, Cerebro를 포함한 완전한 Elasticsearch 테스트 환경을 제공합니다.

## 포함된 서비스

- **Elasticsearch 8.12.1**: 검색 엔진 (포트: 9200, 9300)
- **Kibana 8.12.1**: 데이터 시각화 도구 (포트: 5601)
- **Cerebro 0.9.4**: Elasticsearch 클러스터 모니터링 도구 (포트: 9000)

## 시작하기

### 1. 스택 시작
```bash
docker-compose up -d
```

### 2. 서비스 확인
```bash
docker-compose ps
```

### 3. 로그 확인
```bash
# 모든 서비스 로그
docker-compose logs -f

# 특정 서비스 로그
docker-compose logs -f elasticsearch
docker-compose logs -f kibana
docker-compose logs -f cerebro
```

## 접속 URL

- **Elasticsearch**: http://localhost:9200
- **Kibana**: http://localhost:5601
- **Cerebro**: http://localhost:9000

## 기본 테스트

### Elasticsearch 상태 확인
```bash
curl http://localhost:9200/_cluster/health?pretty
```

### 인덱스 생성 테스트
```bash
curl -X PUT "localhost:9200/test-index" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
'
```

### 문서 추가 테스트
```bash
curl -X POST "localhost:9200/test-index/_doc/1" -H 'Content-Type: application/json' -d'
{
  "title": "테스트 문서",
  "content": "이것은 테스트 문서입니다.",
  "timestamp": "2024-01-01T00:00:00"
}
'
```

### 검색 테스트
```bash
curl -X GET "localhost:9200/test-index/_search?pretty"
```

## 스택 종료

```bash
# 컨테이너 중지
docker-compose down

# 볼륨까지 함께 삭제 (데이터 완전 삭제)
docker-compose down -v
```

## 주의사항

- 이 설정은 **테스트 목적**으로만 사용하세요
- 보안 기능이 비활성화되어 있습니다
- 프로덕션 환경에서는 적절한 보안 설정이 필요합니다
- Elasticsearch는 최소 4GB RAM을 권장합니다

## 문제 해결

### 메모리 부족 오류
Elasticsearch가 시작되지 않는 경우, Docker Desktop의 메모리 할당을 늘려보세요.

### 포트 충돌
포트가 이미 사용 중인 경우, docker-compose.yml에서 포트를 변경하세요.

### 데이터 초기화
데이터를 완전히 초기화하려면:
```bash
docker-compose down -v
docker volume prune
``` 