---
description: OData 다루기
---

# Tutorial 12. Using OData

## 들어가면서

Odata는 UI5에서 데이터 부분을 다루는데 있어서 가장 중요한 부분입니다. 주로 Fiori단에서 웹 개발을 한다면, Odata는 절대적일 정도로 매우 많이 사용되는 프로토콜입니다. 일단 가볍게 Odata란 무엇인지 알아보겠습니다. [위키백과](https://en.wikipedia.org/wiki/Open_Data_Protocol) 에서 Odata의 정의를 찾아보면 이렇습니다.

![https://www.nuget.org/profiles/OData/avatar?imageSize=512](../../.gitbook/assets/image%20%283%29.png)

## Odata의 정의

 단순하고 표준적인 방식으로 쿼리를 쓸수 있고, Restful API를 생성하고 사용할 수 있는 개방형 프로토콜. Odata는 데이터를 주고받는데 있어서 필요한 일종의 규약 중 하나입니다. SAP 외에도 .Net에도 Odata는 사용될 만큼 널리 사용되는 프로토콜 양식 중 하나입니다. 이 기술은 HTTP, ATOM/XML, JSON을 기반으로 만들어졌습니다. 그리고 다른 REST 기반 웹 서비스보다 유연하며 데이터 소스, 애플리케이션, 서비스 및 클라이언트 간의 쉬운 상호 운용성을 위해 데이터 및 데이터 모델을 설명하는 균일한 방법을 제공합니다.

## Odata를 왜 쓰는가?

이번 설명은 이 [링크](https://www.progress.com/blogs/what-is-odata-rest-easy-with-our-quick-guide)를 참고했습니다.

웹 브라우저와 모바일 앱과 같은 다양한 서비스는 방대한 양의 데이터를 사용합니다. 그리고 이를 구성하는 코드 역시 제각각이지요. Odata의 역할은 서로 다른 모든 소스에서 상호 연결된 데이터 에코시스템을 가져와 기존 웹 표준을 기반으로 단순하고 뛰어난 데이터 연결은 가능하게 하기 위함입니다. 이러한 표준을 통해 사용자 지정 응용 프로그램, 클라우드 스토리지, 컨텐츠 관리에 이르기까지 보다 더 좋은 효율성이 향상되지요.

Odata의 가장 큰 장점은 기본적으로 표준화된 REST 인터페이스를 사용하는 것입니다. 따라서 Android, iOS, 다른 비즈니스 웹 서비스 들과 인터페이스 할 때, RESTful 인터페이스를 사용할 수 있습니다. 즉, 호환성이 높다라는 것이지요. 또한, MS나 Critrix System, IBM, SAP 등 여러 회사들의 참여와 협력하였습니다.

#### \*부록 기존 SAP 시스템의 통신방법  

이번 설명은 이 [링크](https://boy0.tistory.com/158)를 참고했습니다. 

UI5는 SAP라는 회사에서 만든 라이브러리이기 때문에, 저희는 엔터프라이즈 시장에서 어떤 통신방법이 사용됬는지 알아놓으면 좋습니다. SAP은 1대 1 독립적인 연결 방식으로 외부시스템과 통합을 이루었습니다. 만약, 하나의 어플리케이션이 두개의 다른 플랫폼에서 사용된다면, SAP에선 두개의 독립된 인터페이스가 필요했지요. \(EX JCO, RFC 등등..\) 하지만 Odata를 사용하면 복잡한 비즈니스가 요구하는 여러 환경에서 공통으로 이용가능한 서비스를 만들 수 있습니다. 또한, Odata를 사용할 때 비용도 들지 않고 라이센스 문제도 없어서 SAP 외부 개발자는 원하는 데이터를 제공하는 서비스를 호출하기만 하면 됩니다.

## Odata 구조

이제 본격적으로 Odata 구조에 대해서 알아봅시다. Odata를 대표하는 하나의 예제가 있습니다. 바로[ Northwind](https://services.odata.org/V2/Northwind/Northwind.svc/)이지요.아래 소스를 보시면 Northwind의 예시를 보실 수 있습니다. 혹시 이 소스를 보시다가 흥미로운 점을 보셨나요? Default라는 타이틀 아래 여러 Collection 단위로 XML이 나눠져 있는 점을 볼 수 있습니다. Collection은 Odata의 EntitySet을 이루는 하나의 단위입니다. Odata는 각 Entity들의 집합입니다. 하나의 Entity는 전달하고자하는 정보의 기본 정보\(타입, 구성 등등\)이 있습니다. 이를 통해 이 API를 호출하면 저희는 어떠한 정보로 API가 구성되어 있고, 어떻게 활용할 수 있는지 알 수 있습니다.

```markup
<?xml version="1.0"?>
<service xmlns:atom="http://www.w3.org/2005/Atom" xmlns:app="http://www.w3.org/2007/app" xmlns="http://www.w3.org/2007/app" xml:base="https://services.odata.org/V2/Northwind/Northwind.svc/">
  <workspace>
    <atom:title>Default</atom:title>
    <collection href="Categories">
      <atom:title>Categories</atom:title>
    </collection>
    <collection href="CustomerDemographics">
      <atom:title>CustomerDemographics</atom:title>
    </collection>
    <collection href="Customers">
      <atom:title>Customers</atom:title>
    </collection>
    <collection href="Employees">
      <atom:title>Employees</atom:title>
    </collection>
    <collection href="Order_Details">
      <atom:title>Order_Details</atom:title>
    </collection>
    <collection href="Orders">
      <atom:title>Orders</atom:title>
    </collection>
    <collection href="Products">
      <atom:title>Products</atom:title>
    </collection>
    <collection href="Regions">
      <atom:title>Regions</atom:title>
    </collection>
    <collection href="Shippers">
      <atom:title>Shippers</atom:title>
    </collection>
    <collection href="Suppliers">
      <atom:title>Suppliers</atom:title>
    </collection>
    <collection href="Territories">
      <atom:title>Territories</atom:title>
    </collection>
    <collection href="Alphabetical_list_of_products">
      <atom:title>Alphabetical_list_of_products</atom:title>
    </collection>
    <collection href="Category_Sales_for_1997">
      <atom:title>Category_Sales_for_1997</atom:title>
    </collection>
    <collection href="Current_Product_Lists">
      <atom:title>Current_Product_Lists</atom:title>
    </collection>
    <collection href="Customer_and_Suppliers_by_Cities">
      <atom:title>Customer_and_Suppliers_by_Cities</atom:title>
    </collection>
    <collection href="Invoices">
      <atom:title>Invoices</atom:title>
    </collection>
    <collection href="Order_Details_Extendeds">
      <atom:title>Order_Details_Extendeds</atom:title>
    </collection>
    <collection href="Order_Subtotals">
      <atom:title>Order_Subtotals</atom:title>
    </collection>
    <collection href="Orders_Qries">
      <atom:title>Orders_Qries</atom:title>
    </collection>
    <collection href="Product_Sales_for_1997">
      <atom:title>Product_Sales_for_1997</atom:title>
    </collection>
    <collection href="Products_Above_Average_Prices">
      <atom:title>Products_Above_Average_Prices</atom:title>
    </collection>
    <collection href="Products_by_Categories">
      <atom:title>Products_by_Categories</atom:title>
    </collection>
    <collection href="Sales_by_Categories">
      <atom:title>Sales_by_Categories</atom:title>
    </collection>
    <collection href="Sales_Totals_by_Amounts">
      <atom:title>Sales_Totals_by_Amounts</atom:title>
    </collection>
    <collection href="Summary_of_Sales_by_Quarters">
      <atom:title>Summary_of_Sales_by_Quarters</atom:title>
    </collection>
    <collection href="Summary_of_Sales_by_Years">
      <atom:title>Summary_of_Sales_by_Years</atom:title>
    </collection>
  </workspace>
</service>
```

아직 저희는 이 API의 실제 데이터에 접근하진 않았습니다. 최상위 루트 정보만을 제공받은 것 뿐이지요. 이제부터 차근차근 접근하도록 해보겠습니다.

`GET:`[`https://services.odata.org/Northwind/Northwind.svc/Customers`](https://services.odata.org/Northwind/Northwind.svc/Customers)\`\`

위의 url 을 Postman에서 GET 방식으로 호출을 한다면 Customers EntitySet에 매핑된 데이터들을 로드할 수 있습니다.

```markup
<?xml version="1.0" encoding="utf-8"?>
<feed xml:base="https://services.odata.org/Northwind/Northwind.svc/" xmlns="http://www.w3.org/2005/Atom" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">
    <id>https://services.odata.org/Northwind/Northwind.svc/Customers</id>
    <title type="text">Customers</title>
    <updated>2020-05-12T12:04:37Z</updated>
    <link rel="self" title="Customers" href="Customers" />
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('ALFKI')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('ALFKI')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('ALFKI')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>ALFKI</d:CustomerID>
                <d:CompanyName>Alfreds Futterkiste</d:CompanyName>
                <d:ContactName>Maria Anders</d:ContactName>
                <d:ContactTitle>Sales Representative</d:ContactTitle>
                <d:Address>Obere Str. 57</d:Address>
                <d:City>Berlin</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>12209</d:PostalCode>
                <d:Country>Germany</d:Country>
                <d:Phone>030-0074321</d:Phone>
                <d:Fax>030-0076545</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('ANATR')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('ANATR')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('ANATR')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('ANATR')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>ANATR</d:CustomerID>
                <d:CompanyName>Ana Trujillo Emparedados y helados</d:CompanyName>
                <d:ContactName>Ana Trujillo</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>Avda. de la Constitución 2222</d:Address>
                <d:City>México D.F.</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>05021</d:PostalCode>
                <d:Country>Mexico</d:Country>
                <d:Phone>(5) 555-4729</d:Phone>
                <d:Fax>(5) 555-3745</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('ANTON')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('ANTON')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('ANTON')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('ANTON')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>ANTON</d:CustomerID>
                <d:CompanyName>Antonio Moreno Taquería</d:CompanyName>
                <d:ContactName>Antonio Moreno</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>Mataderos  2312</d:Address>
                <d:City>México D.F.</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>05023</d:PostalCode>
                <d:Country>Mexico</d:Country>
                <d:Phone>(5) 555-3932</d:Phone>
                <d:Fax m:null="true" />
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('AROUT')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('AROUT')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('AROUT')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('AROUT')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>AROUT</d:CustomerID>
                <d:CompanyName>Around the Horn</d:CompanyName>
                <d:ContactName>Thomas Hardy</d:ContactName>
                <d:ContactTitle>Sales Representative</d:ContactTitle>
                <d:Address>120 Hanover Sq.</d:Address>
                <d:City>London</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>WA1 1DP</d:PostalCode>
                <d:Country>UK</d:Country>
                <d:Phone>(171) 555-7788</d:Phone>
                <d:Fax>(171) 555-6750</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BERGS')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BERGS')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BERGS')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BERGS')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BERGS</d:CustomerID>
                <d:CompanyName>Berglunds snabbköp</d:CompanyName>
                <d:ContactName>Christina Berglund</d:ContactName>
                <d:ContactTitle>Order Administrator</d:ContactTitle>
                <d:Address>Berguvsvägen  8</d:Address>
                <d:City>Luleå</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>S-958 22</d:PostalCode>
                <d:Country>Sweden</d:Country>
                <d:Phone>0921-12 34 65</d:Phone>
                <d:Fax>0921-12 34 67</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BLAUS')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BLAUS')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BLAUS')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BLAUS')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BLAUS</d:CustomerID>
                <d:CompanyName>Blauer See Delikatessen</d:CompanyName>
                <d:ContactName>Hanna Moos</d:ContactName>
                <d:ContactTitle>Sales Representative</d:ContactTitle>
                <d:Address>Forsterstr. 57</d:Address>
                <d:City>Mannheim</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>68306</d:PostalCode>
                <d:Country>Germany</d:Country>
                <d:Phone>0621-08460</d:Phone>
                <d:Fax>0621-08924</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BLONP')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BLONP')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BLONP')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BLONP')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BLONP</d:CustomerID>
                <d:CompanyName>Blondesddsl père et fils</d:CompanyName>
                <d:ContactName>Frédérique Citeaux</d:ContactName>
                <d:ContactTitle>Marketing Manager</d:ContactTitle>
                <d:Address>24, place Kléber</d:Address>
                <d:City>Strasbourg</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>67000</d:PostalCode>
                <d:Country>France</d:Country>
                <d:Phone>88.60.15.31</d:Phone>
                <d:Fax>88.60.15.32</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BOLID')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BOLID')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BOLID')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BOLID')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BOLID</d:CustomerID>
                <d:CompanyName>Bólido Comidas preparadas</d:CompanyName>
                <d:ContactName>Martín Sommer</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>C/ Araquil, 67</d:Address>
                <d:City>Madrid</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>28023</d:PostalCode>
                <d:Country>Spain</d:Country>
                <d:Phone>(91) 555 22 82</d:Phone>
                <d:Fax>(91) 555 91 99</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BONAP')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BONAP')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BONAP')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BONAP')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BONAP</d:CustomerID>
                <d:CompanyName>Bon app'</d:CompanyName>
                <d:ContactName>Laurence Lebihan</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>12, rue des Bouchers</d:Address>
                <d:City>Marseille</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>13008</d:PostalCode>
                <d:Country>France</d:Country>
                <d:Phone>91.24.45.40</d:Phone>
                <d:Fax>91.24.45.41</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BOTTM')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BOTTM')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BOTTM')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BOTTM')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BOTTM</d:CustomerID>
                <d:CompanyName>Bottom-Dollar Markets</d:CompanyName>
                <d:ContactName>Elizabeth Lincoln</d:ContactName>
                <d:ContactTitle>Accounting Manager</d:ContactTitle>
                <d:Address>23 Tsawassen Blvd.</d:Address>
                <d:City>Tsawassen</d:City>
                <d:Region>BC</d:Region>
                <d:PostalCode>T2F 8M4</d:PostalCode>
                <d:Country>Canada</d:Country>
                <d:Phone>(604) 555-4729</d:Phone>
                <d:Fax>(604) 555-3745</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('BSBEV')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('BSBEV')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('BSBEV')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('BSBEV')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>BSBEV</d:CustomerID>
                <d:CompanyName>B's Beverages</d:CompanyName>
                <d:ContactName>Victoria Ashworth</d:ContactName>
                <d:ContactTitle>Sales Representative</d:ContactTitle>
                <d:Address>Fauntleroy Circus</d:Address>
                <d:City>London</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>EC2 5NT</d:PostalCode>
                <d:Country>UK</d:Country>
                <d:Phone>(171) 555-1212</d:Phone>
                <d:Fax m:null="true" />
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('CACTU')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('CACTU')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('CACTU')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('CACTU')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>CACTU</d:CustomerID>
                <d:CompanyName>Cactus Comidas para llevar</d:CompanyName>
                <d:ContactName>Patricio Simpson</d:ContactName>
                <d:ContactTitle>Sales Agent</d:ContactTitle>
                <d:Address>Cerrito 333</d:Address>
                <d:City>Buenos Aires</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>1010</d:PostalCode>
                <d:Country>Argentina</d:Country>
                <d:Phone>(1) 135-5555</d:Phone>
                <d:Fax>(1) 135-4892</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('CENTC')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('CENTC')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('CENTC')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('CENTC')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>CENTC</d:CustomerID>
                <d:CompanyName>Centro comercial Moctezuma</d:CompanyName>
                <d:ContactName>Francisco Chang</d:ContactName>
                <d:ContactTitle>Marketing Manager</d:ContactTitle>
                <d:Address>Sierras de Granada 9993</d:Address>
                <d:City>México D.F.</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>05022</d:PostalCode>
                <d:Country>Mexico</d:Country>
                <d:Phone>(5) 555-3392</d:Phone>
                <d:Fax>(5) 555-7293</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('CHOPS')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('CHOPS')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('CHOPS')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('CHOPS')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>CHOPS</d:CustomerID>
                <d:CompanyName>Chop-suey Chinese</d:CompanyName>
                <d:ContactName>Yang Wang</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>Hauptstr. 29</d:Address>
                <d:City>Bern</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>3012</d:PostalCode>
                <d:Country>Switzerland</d:Country>
                <d:Phone>0452-076545</d:Phone>
                <d:Fax m:null="true" />
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('COMMI')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('COMMI')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('COMMI')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('COMMI')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>COMMI</d:CustomerID>
                <d:CompanyName>Comércio Mineiro</d:CompanyName>
                <d:ContactName>Pedro Afonso</d:ContactName>
                <d:ContactTitle>Sales Associate</d:ContactTitle>
                <d:Address>Av. dos Lusíadas, 23</d:Address>
                <d:City>Sao Paulo</d:City>
                <d:Region>SP</d:Region>
                <d:PostalCode>05432-043</d:PostalCode>
                <d:Country>Brazil</d:Country>
                <d:Phone>(11) 555-7647</d:Phone>
                <d:Fax m:null="true" />
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('CONSH')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('CONSH')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('CONSH')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('CONSH')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>CONSH</d:CustomerID>
                <d:CompanyName>Consolidated Holdings</d:CompanyName>
                <d:ContactName>Elizabeth Brown</d:ContactName>
                <d:ContactTitle>Sales Representative</d:ContactTitle>
                <d:Address>Berkeley Gardens 12  Brewery</d:Address>
                <d:City>London</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>WX1 6LT</d:PostalCode>
                <d:Country>UK</d:Country>
                <d:Phone>(171) 555-2282</d:Phone>
                <d:Fax>(171) 555-9199</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('DRACD')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('DRACD')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('DRACD')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('DRACD')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>DRACD</d:CustomerID>
                <d:CompanyName>Drachenblut Delikatessen</d:CompanyName>
                <d:ContactName>Sven Ottlieb</d:ContactName>
                <d:ContactTitle>Order Administrator</d:ContactTitle>
                <d:Address>Walserweg 21</d:Address>
                <d:City>Aachen</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>52066</d:PostalCode>
                <d:Country>Germany</d:Country>
                <d:Phone>0241-039123</d:Phone>
                <d:Fax>0241-059428</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('DUMON')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('DUMON')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('DUMON')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('DUMON')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>DUMON</d:CustomerID>
                <d:CompanyName>Du monde entier</d:CompanyName>
                <d:ContactName>Janine Labrune</d:ContactName>
                <d:ContactTitle>Owner</d:ContactTitle>
                <d:Address>67, rue des Cinquante Otages</d:Address>
                <d:City>Nantes</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>44000</d:PostalCode>
                <d:Country>France</d:Country>
                <d:Phone>40.67.88.88</d:Phone>
                <d:Fax>40.67.89.89</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('EASTC')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('EASTC')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('EASTC')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('EASTC')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>EASTC</d:CustomerID>
                <d:CompanyName>Eastern Connection</d:CompanyName>
                <d:ContactName>Ann Devon</d:ContactName>
                <d:ContactTitle>Sales Agent</d:ContactTitle>
                <d:Address>35 King George</d:Address>
                <d:City>London</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>WX3 6FW</d:PostalCode>
                <d:Country>UK</d:Country>
                <d:Phone>(171) 555-0297</d:Phone>
                <d:Fax>(171) 555-3373</d:Fax>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://services.odata.org/Northwind/Northwind.svc/Customers('ERNSH')</id>
        <category term="NorthwindModel.Customer" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/scheme" />
        <link rel="edit" title="Customer" href="Customers('ERNSH')" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Orders" type="application/atom+xml;type=feed" title="Orders" href="Customers('ERNSH')/Orders" />
        <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/CustomerDemographics" type="application/atom+xml;type=feed" title="CustomerDemographics" href="Customers('ERNSH')/CustomerDemographics" />
        <title />
        <updated>2020-05-12T12:04:37Z</updated>
        <author>
            <name />
        </author>
        <content type="application/xml">
            <m:properties>
                <d:CustomerID>ERNSH</d:CustomerID>
                <d:CompanyName>Ernst Handel</d:CompanyName>
                <d:ContactName>Roland Mendel</d:ContactName>
                <d:ContactTitle>Sales Manager</d:ContactTitle>
                <d:Address>Kirchgasse 6</d:Address>
                <d:City>Graz</d:City>
                <d:Region m:null="true" />
                <d:PostalCode>8010</d:PostalCode>
                <d:Country>Austria</d:Country>
                <d:Phone>7675-3425</d:Phone>
                <d:Fax>7675-3426</d:Fax>
            </m:properties>
        </content>
    </entry>
    <link rel="next" href="https://services.odata.org/Northwind/Northwind.svc/Customers?$skiptoken='ERNSH'" />
</feed>
```

Odata는 XML 뿐만 아니라 JSON 포맷도 제공합니다.

[`https://services.odata.org/Northwind/Northwind.svc/Customers?$format=json`](https://services.odata.org/Northwind/Northwind.svc/Customers?$format=json)\`\`

  위의 URL을 호출하시면, JSON 방식으로 변환된 response를 받을 수 있습니다.

```javascript
{
    "odata.metadata": "https://services.odata.org/Northwind/Northwind.svc/$metadata#Customers",
    "value": [
        {
            "CustomerID": "ALFKI",
            "CompanyName": "Alfreds Futterkiste",
            "ContactName": "Maria Anders",
            "ContactTitle": "Sales Representative",
            "Address": "Obere Str. 57",
            "City": "Berlin",
            "Region": null,
            "PostalCode": "12209",
            "Country": "Germany",
            "Phone": "030-0074321",
            "Fax": "030-0076545"
        },
        {
            "CustomerID": "ANATR",
            "CompanyName": "Ana Trujillo Emparedados y helados",
            "ContactName": "Ana Trujillo",
            "ContactTitle": "Owner",
            "Address": "Avda. de la Constitución 2222",
            "City": "México D.F.",
            "Region": null,
            "PostalCode": "05021",
            "Country": "Mexico",
            "Phone": "(5) 555-4729",
            "Fax": "(5) 555-3745"
        },
        {
            "CustomerID": "ANTON",
            "CompanyName": "Antonio Moreno Taquería",
            "ContactName": "Antonio Moreno",
            "ContactTitle": "Owner",
            "Address": "Mataderos  2312",
            "City": "México D.F.",
            "Region": null,
            "PostalCode": "05023",
            "Country": "Mexico",
            "Phone": "(5) 555-3932",
            "Fax": null
        },
        {
            "CustomerID": "AROUT",
            "CompanyName": "Around the Horn",
            "ContactName": "Thomas Hardy",
            "ContactTitle": "Sales Representative",
            "Address": "120 Hanover Sq.",
            "City": "London",
            "Region": null,
            "PostalCode": "WA1 1DP",
            "Country": "UK",
            "Phone": "(171) 555-7788",
            "Fax": "(171) 555-6750"
        },
        {
            "CustomerID": "BERGS",
            "CompanyName": "Berglunds snabbköp",
            "ContactName": "Christina Berglund",
            "ContactTitle": "Order Administrator",
            "Address": "Berguvsvägen  8",
            "City": "Luleå",
            "Region": null,
            "PostalCode": "S-958 22",
            "Country": "Sweden",
            "Phone": "0921-12 34 65",
            "Fax": "0921-12 34 67"
        },
        {
            "CustomerID": "BLAUS",
            "CompanyName": "Blauer See Delikatessen",
            "ContactName": "Hanna Moos",
            "ContactTitle": "Sales Representative",
            "Address": "Forsterstr. 57",
            "City": "Mannheim",
            "Region": null,
            "PostalCode": "68306",
            "Country": "Germany",
            "Phone": "0621-08460",
            "Fax": "0621-08924"
        },
        {
            "CustomerID": "BLONP",
            "CompanyName": "Blondesddsl père et fils",
            "ContactName": "Frédérique Citeaux",
            "ContactTitle": "Marketing Manager",
            "Address": "24, place Kléber",
            "City": "Strasbourg",
            "Region": null,
            "PostalCode": "67000",
            "Country": "France",
            "Phone": "88.60.15.31",
            "Fax": "88.60.15.32"
        },
        {
            "CustomerID": "BOLID",
            "CompanyName": "Bólido Comidas preparadas",
            "ContactName": "Martín Sommer",
            "ContactTitle": "Owner",
            "Address": "C/ Araquil, 67",
            "City": "Madrid",
            "Region": null,
            "PostalCode": "28023",
            "Country": "Spain",
            "Phone": "(91) 555 22 82",
            "Fax": "(91) 555 91 99"
        },
        {
            "CustomerID": "BONAP",
            "CompanyName": "Bon app'",
            "ContactName": "Laurence Lebihan",
            "ContactTitle": "Owner",
            "Address": "12, rue des Bouchers",
            "City": "Marseille",
            "Region": null,
            "PostalCode": "13008",
            "Country": "France",
            "Phone": "91.24.45.40",
            "Fax": "91.24.45.41"
        },
        {
            "CustomerID": "BOTTM",
            "CompanyName": "Bottom-Dollar Markets",
            "ContactName": "Elizabeth Lincoln",
            "ContactTitle": "Accounting Manager",
            "Address": "23 Tsawassen Blvd.",
            "City": "Tsawassen",
            "Region": "BC",
            "PostalCode": "T2F 8M4",
            "Country": "Canada",
            "Phone": "(604) 555-4729",
            "Fax": "(604) 555-3745"
        },
        {
            "CustomerID": "BSBEV",
            "CompanyName": "B's Beverages",
            "ContactName": "Victoria Ashworth",
            "ContactTitle": "Sales Representative",
            "Address": "Fauntleroy Circus",
            "City": "London",
            "Region": null,
            "PostalCode": "EC2 5NT",
            "Country": "UK",
            "Phone": "(171) 555-1212",
            "Fax": null
        },
        {
            "CustomerID": "CACTU",
            "CompanyName": "Cactus Comidas para llevar",
            "ContactName": "Patricio Simpson",
            "ContactTitle": "Sales Agent",
            "Address": "Cerrito 333",
            "City": "Buenos Aires",
            "Region": null,
            "PostalCode": "1010",
            "Country": "Argentina",
            "Phone": "(1) 135-5555",
            "Fax": "(1) 135-4892"
        },
        {
            "CustomerID": "CENTC",
            "CompanyName": "Centro comercial Moctezuma",
            "ContactName": "Francisco Chang",
            "ContactTitle": "Marketing Manager",
            "Address": "Sierras de Granada 9993",
            "City": "México D.F.",
            "Region": null,
            "PostalCode": "05022",
            "Country": "Mexico",
            "Phone": "(5) 555-3392",
            "Fax": "(5) 555-7293"
        },
        {
            "CustomerID": "CHOPS",
            "CompanyName": "Chop-suey Chinese",
            "ContactName": "Yang Wang",
            "ContactTitle": "Owner",
            "Address": "Hauptstr. 29",
            "City": "Bern",
            "Region": null,
            "PostalCode": "3012",
            "Country": "Switzerland",
            "Phone": "0452-076545",
            "Fax": null
        },
        {
            "CustomerID": "COMMI",
            "CompanyName": "Comércio Mineiro",
            "ContactName": "Pedro Afonso",
            "ContactTitle": "Sales Associate",
            "Address": "Av. dos Lusíadas, 23",
            "City": "Sao Paulo",
            "Region": "SP",
            "PostalCode": "05432-043",
            "Country": "Brazil",
            "Phone": "(11) 555-7647",
            "Fax": null
        },
        {
            "CustomerID": "CONSH",
            "CompanyName": "Consolidated Holdings",
            "ContactName": "Elizabeth Brown",
            "ContactTitle": "Sales Representative",
            "Address": "Berkeley Gardens 12  Brewery",
            "City": "London",
            "Region": null,
            "PostalCode": "WX1 6LT",
            "Country": "UK",
            "Phone": "(171) 555-2282",
            "Fax": "(171) 555-9199"
        },
        {
            "CustomerID": "DRACD",
            "CompanyName": "Drachenblut Delikatessen",
            "ContactName": "Sven Ottlieb",
            "ContactTitle": "Order Administrator",
            "Address": "Walserweg 21",
            "City": "Aachen",
            "Region": null,
            "PostalCode": "52066",
            "Country": "Germany",
            "Phone": "0241-039123",
            "Fax": "0241-059428"
        },
        {
            "CustomerID": "DUMON",
            "CompanyName": "Du monde entier",
            "ContactName": "Janine Labrune",
            "ContactTitle": "Owner",
            "Address": "67, rue des Cinquante Otages",
            "City": "Nantes",
            "Region": null,
            "PostalCode": "44000",
            "Country": "France",
            "Phone": "40.67.88.88",
            "Fax": "40.67.89.89"
        },
        {
            "CustomerID": "EASTC",
            "CompanyName": "Eastern Connection",
            "ContactName": "Ann Devon",
            "ContactTitle": "Sales Agent",
            "Address": "35 King George",
            "City": "London",
            "Region": null,
            "PostalCode": "WX3 6FW",
            "Country": "UK",
            "Phone": "(171) 555-0297",
            "Fax": "(171) 555-3373"
        },
        {
            "CustomerID": "ERNSH",
            "CompanyName": "Ernst Handel",
            "ContactName": "Roland Mendel",
            "ContactTitle": "Sales Manager",
            "Address": "Kirchgasse 6",
            "City": "Graz",
            "Region": null,
            "PostalCode": "8010",
            "Country": "Austria",
            "Phone": "7675-3425",
            "Fax": "7675-3426"
        }
    ],
    "odata.nextLink": "Customers?$skiptoken='ERNSH'"
}
```

### Odata EntityType 키값으로 데이터 조회하기

Odata Entityset은 키값을 가지고 있습니다. 저희는 이 키값을 통해 원하는 데이터를 효과적으로 추출할 수 있습니다. 

[`https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')?$format=json`](https://services.odata.org/Northwind/Northwind.svc/Customers%28'ALFKI'%29?$format=json)\`\`

위의 링크를 호출하시면, Customers 엔티티 그룹의 키값\('ALFKI'\)에 해당하는 값을 호출합니다.

```javascript
{
    "odata.metadata": "https://services.odata.org/Northwind/Northwind.svc/$metadata#Customers/@Element",
    "CustomerID": "ALFKI",
    "CompanyName": "Alfreds Futterkiste",
    "ContactName": "Maria Anders",
    "ContactTitle": "Sales Representative",
    "Address": "Obere Str. 57",
    "City": "Berlin",
    "Region": null,
    "PostalCode": "12209",
    "Country": "Germany",
    "Phone": "030-0074321",
    "Fax": "030-0076545"
}
```

## UI5에서 Odata 사용하기 

UI5는 물론 fetch나 xmlHttpRequest와 같은 AJAX 함수를 이용하여 데이터를 주고받을 수 있지만, 기본적으로 OdataModel을 활용하여 CRUD 기능을 수행합니다.

