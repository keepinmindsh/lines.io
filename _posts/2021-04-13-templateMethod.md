---
title:  "Pattern - Template Method"
excerpt: "Gof Design Pattern"

categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 너무 욕심내지 마라. 천천히 그리고 꾸준히 하는게 답이다.

# 의도 

***

객체의 연산이 알고리즘의 뼈대 만들 정의하고 각 단계에서 수행할 구체적 처리는 서브 클래스 쪽에서 미룹니다. 알고리즘의 구조 자체는 그대로 놔둔 채 알고리즘 각 단계의 처리를 서브 클래스에서 처리할 수 있게 합니다.

# 동기 

***

Application 클래스와 Draw 클래스를 제공하는 응용 프로그램 프레임워크를 생각해 봅시다. Application 클래스는 파일이나 특정한 외부 형식으로 저장된 문서를
열 수 있고, Document 객체를 파일에서 읽은 문서에 정보를 나타냅니다.
특정한 요구나 따라서 프레임워크 정의한 Application 클래스와 Document 클래스를 상속한 서브 클래스를 정의하여 새로운 응용 프로그램을 구축할 수 있을 것입니다.
예를 들어, 그림 그리기 응용 프로그램은 DrawApplication 클래스와 DrawDocument 클래스를 정의할 수 있을 것이고, 스프레드 시트 응용프로그램은 SpreadsheetApplication 클래스와
SpreadsheetDocument 클래스를 정의할 수 있습니다. 이와 같은 패턴을 구현할 때 사용하는 것이 탬플릿 메서드라고 합니다.

# 활용성 

***

![](https://keepinmindsh.github.io/lines/assets/img/templatemethod.png){: .align-center}

- 어떤 한 알고리즘을 이루는 부분 중 변하지 않는 부분을 한 번 정의해 놓고 다양해질 수 있는 부분을 서브 클래스에서 정의할 수 있도록 남겨주고자 할 때
- 서브 클래스 사이의 공통적인 행동을 추출하여 하나의 공통 클래스에 몰아둠으로써 코드 중복을 피하고 싶을 때.
먼저, 기존 코드에서 나타나는 차이점을 뽑아 이를 별도의 새로운 연산들로 구분해 놓습니다. 그런 뒤 달라진 코드 부분을 이 새로운 연산을 호출 하는 템플릿 메서드로 대체하는 것입니다.
- 서브 클래스의 확장을 제어할 수 있습니다. 템플릿 메서드가 어떤 특정한 시점에 "훅(hook)" 연산을 호출하도록 정의함으로써, 그 특정 시점에서만 확장되도록 합니다.

# 항목에 대한 설명 

***

- AbstractClass  
*Application*  
서브 클래스들이 재정의를 통해 구현해야 하는 알고리즘 처리 단계 내의 기본 연산을 정의합니다.
그리고 알고리즘의 뼈대를 정의하는 템플릿 메서드를 구현합니다. 템플릿 메서드는 AbstractClass 에 정의된 연산 또는 다른 객체 뿐만 아니라 기본 연산도 호출합니다.

- ConcreteClass  
*MyApplication*  
서브 클래스마다 달라진 알고리즘 처리 단계를 수행하기 위한 기본 연산을 구현합니다.

# 협력방법

***

- ConcreteClass는 AbstractClass를 통하여 알고리즘의 변하지 않는 처리 단계를 구현합니다.

# 결과 

***

- 템플릿 메서드는 "할리우드 원칙"이라는 제어 역전의 구조를 끌어냅니다. "전화하지 마세요. 우리가 연락할게요." 라는 것입니다.
다시 말해, 부모 클래스는 서브 클래스에 정의된 연산을 호출할 수 있지만 반대 방향의 호출은 아닙니다.
- 구체 연산 : ConcreteClass 나 사용자 클래스에 정의된 연산
- AbstractClass 구체 연산 : 서브 클래스에서 일반적으로 유용한 연산
- 기본 연산 : 추상화된 연산
- 팩토리 메서드
- 훅 연산 : 필요하다면 서브클래스에서 확장할 수 있는 기본 행동을 제공하는 연산. 기본적으로는 아무 내용도 정의하지 않습니다.


템플릿 메서드 패턴에서는 어떤 연산이 훅 연산인지(오버라이드 가능한지) 추상연산인지(꼭 오버라이드 해야하는지)를 지정해 두는 것이 대단히 중요합니다.
훅 연산은 나중에 재정의할 수 있고, 재정의 하지 않을 수도 있는 메서드이고, 추상 연산은 반드시 재정의해야 하는 연산입니다.
추상할 클래스를 효과적으로 재사용하기 위해서, 서브 클래스 작성자는 어떤 연산들이 오버라이드용으로 설계되었는지를 정확하게 이해하고 있어야 합니다.
{: .notice--info}

# 코드 예제 -- 기본 샘플 

***

```java

public class Caller {

  public static void main(String[] args) {
      DBTemplate dbTemplate = new OracleDB();

      List<Map> resutList = (List<Map>)dbTemplate.execute("SELECT * FROM TB_ZZ_USER");

      resutList.forEach(value -> {
          System.out.println(value.get("USER_ID"));
      });
  }
}

```

```java

public abstract class DBTemplate {

  public Object selectQuery(String sql){
      return execute(sql);
  }

  public List<Map> execute(String queryStatment){

      ResultSet resultSet = null;
      List<Map> resultList = null;
      Connection connection = null;
      PreparedStatement preparedStatement = null;

      try {

          String[] connectionInfo = getDBInformation();

          connection = getConnection(connectionInfo);

          preparedStatement = executeStatement(connection, queryStatment);

          resultSet = getResultSet(preparedStatement);

          resultList = getResultMapRows(resultSet);

      }catch (Exception ex){
          ex.printStackTrace();
      }finally {
          try{
              if(connection != null ) releaseConnection(connection);
              if(preparedStatement != null ) releasePrepareStatement(preparedStatement);
              if(resultSet != null ) releaseResultSet(resultSet);
          }catch (Exception ex){
              ex.printStackTrace();
          }

      }

      return resultList;
  }

  /**
    * ResultSet을 Row마다 Map에 저장후 List에 다시 저장.
    * @param rs DB에서 가져온 ResultSet
    * @return Listt<map> 형태로 리턴
    * @throws Exception Collection
    */
  private List<Map> getResultMapRows(ResultSet rs) throws Exception
  {
      // ResultSet 의 MetaData를 가져온다.
      ResultSetMetaData metaData = rs.getMetaData();
      // ResultSet 의 Column의 갯수를 가져온다.
      int sizeOfColumn = metaData.getColumnCount();

      List<Map> list = new ArrayList<Map>();
      Map<String, Object> map;
      String column;
      // rs의 내용을 돌려준다.
      while (rs.next())
      {
          // 내부에서 map을 초기화
          map = new HashMap<String, Object>();
          // Column의 갯수만큼 회전
          for (int indexOfcolumn = 0; indexOfcolumn < sizeOfColumn; indexOfcolumn++)
          {
              column = metaData.getColumnName(indexOfcolumn + 1);
              // map에 값을 입력 map.put(columnName, columnName으로 getString)
              map.put(column, rs.getString(column));
          }
          // list에 저장
          list.add(map);
      }
      return list;
  }

  private void releaseConnection(Connection connection) throws SQLException {
      connection.close();
  }

  private void releaseResultSet(ResultSet resultSet) throws SQLException {
      resultSet.close();
  }

  private void releasePrepareStatement(PreparedStatement preparedStatement) throws SQLException {
      preparedStatement.close();
  }

  public abstract String[] getDBInformation();

  public abstract Connection getConnection(String[] connectionInfo) throws SQLException;

  public abstract PreparedStatement executeStatement(Connection connection, String sql) throws SQLException;

  public abstract ResultSet getResultSet(PreparedStatement preparedStatement) throws SQLException;

}

public class OracleDB extends DBTemplate {
  @Override
  public String[] getDBInformation() {
      String jdbcDriver = "jdbc:oracle:thin:@***.***.***.***:***/*****";
      String dbUser = "******";
      String dbPassword = "******";

      String[] resultInfo = {jdbcDriver, dbUser, dbPassword};

      return resultInfo;
  }

  @Override
  public Connection getConnection(String[] connectionInfo) throws SQLException {
      return DriverManager.getConnection(connectionInfo[0], connectionInfo[1], connectionInfo[2]);
  }

  @Override
  public PreparedStatement executeStatement(Connection connection, String sql) throws SQLException {
      return connection.prepareStatement(sql);
  }

  @Override
  public ResultSet getResultSet(PreparedStatement preparedStatement) throws SQLException {
      return preparedStatement.executeQuery();
  }
}

```
