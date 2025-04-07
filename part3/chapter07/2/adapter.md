# 어댑터 패턴

> 클라이언트가 요구하는 인터페이스와 재사용하려는 모듈의 인터페이스가 일치하지 않을 떄 사용할 수 있는 패턴(구조패턴)

- 어댑터 패턴을 통해 개방 폐쇄 원칙을 따를 수 있다.

## 구현

- 객체 조립

```java
public class SearchServiceTolrAdapter implements SearchService {
	private TolrClient tolrClient = new TolrClient(); // 조립을 통해 구현

	public SearchResult search(String keyword) {
		TolrQuery tolrQuery = new TolrQuery(keyword);

		QueryResponse response = tolrClient.query(tolrQuery);
		SearchResult result = convertToResult(response);
		// TolrClient의 결과를 클라이언트가 요구하는 타입으로 변한해줌

		return result;
	}

	private SearchResult convertToResult(QueryResponse response) {
		List<TolrDocument> tolrDocs = response.getDocumentList().getDocuments();
		List<SearchDocument> docs = new ArrayList<SearchDocument> ();

		for (TolrDocument tolrDoc: tolrDocs) {
			docs.add(new SearchDocument(tolrDoc.getId(), ...));
		}

		return new SearchResult(docs);
	}
}
```

![어댑터예제](/images/adapter-2.png)

- 객체 상속

```java
public class SearchServiceTolrAdapter extends TolrClient
	implements SearchService {
	// SearchService가 추상 클래스일 경우 단일 상속만을 지원하는 Java 언어에서는 사용 X
		public SearchResult search(String keyword) {
		TolrQuery tolrQuery = new TolrQuery(keyword);

		QueryResponse response = super.query(tolrQuery);
		SearchResult result = convertToResult(response);
		// TolrClient의 결과를 클라이언트가 요구하는 타입으로 변한해줌

		return result;
	}

	private SearchResult convertToResult(QueryResponse response) {
		List<TolrDocument> tolrDocs = response.getDocumentList().getDocuments();
		List<SearchDocument> docs = new ArrayList<SearchDocument> ();

		for (TolrDocument tolrDoc: tolrDocs) {
			docs.add(new SearchDocument(tolrDoc.getId(), ...));
		}

		return new SearchResult(docs);
	}
}
```

![어댑터예제](/images/adapter-1.png)
