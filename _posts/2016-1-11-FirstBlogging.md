---
layout: post
title: Your First Post
published: true
date: 2016-1-11
categories: [test]
tags: [test,sql,go,기타등등]
---

### 처음테스트 입니다.

맥북에서 git 을 연동하여 Atom으로 작업하고 있습니다.
Markdown Preview 를 활용하여 md파일을 작성하고 있습니다.

 * go 언어를 주고 개발하고 있어서 go 하일라이트 테스트를 합니다.
```go
func test(message string ) string {
  return "hello" + message
}
```
* 최근 UNIX_TIMESTAMP를 사용하면서 알게된 사실
아직도 32bit 를 사용하기 때문에 2038년이 계산이 되지 않습니다.

```sql
mysql> SELECT unix_timestamp('2038-1-31 00:00:00')
+--------------------------------------+
| unix_timestamp('2038-1-31 00:00:00') |
+--------------------------------------+
|                                    0 |
+--------------------------------------+
```
