---
title: "분석 데이터 저장소 선택"
description: 
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: b2e5e63982d4b89b95cd28e596d3b882a4a2263e
ms.sourcegitcommit: 90cf2de795e50571d597cfcb9b302e48933e7f18
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2018
---
# <a name="choosing-an-analytical-data-store-in-azure"></a><span data-ttu-id="0fcbb-102">Azure에서 분석 데이터 저장소 선택</span><span class="sxs-lookup"><span data-stu-id="0fcbb-102">Choosing an analytical data store in Azure</span></span>

<span data-ttu-id="0fcbb-103">[빅 데이터](../concepts/big-data.md) 아키텍처에서는 처리된 데이터를 분석 도구로 쿼리할 수 있는 구조화된 형식으로 제공하는 분석 데이터 저장소가 필요한 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-103">In a [big data](../concepts/big-data.md) architecture, there is often a need for an analytical data store that serves processed data in a structured format that can be queried using analytical tools.</span></span> <span data-ttu-id="0fcbb-104">실행 부하 과다 경로 및 실행 부하 미달 경로 데이터의 쿼리를 모두 지원하는 분석 데이터 저장소를 톨틀어 서비스 계층 또는 데이터 서비스 저장소라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-104">Analytical data stores that support querying of both hot-path and cold-path data are collectively referred to as the serving layer, or data serving storage.</span></span>

<span data-ttu-id="0fcbb-105">서비스 계층은 실행 부하 과다 경로 및 실행 부하 미달 경로의 처리된 데이터를 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-105">The serving layer deals with processed data from both the hot path and cold path.</span></span> <span data-ttu-id="0fcbb-106">[람다 아키텍처](../concepts/big-data.md#lambda-architecture)에서 서비스 계층은 증분 방식으로 처리된 데이터를 저장하는 _빠른 서비스_ 계층과 일괄 처리된 출력을 포함하는 _일괄 처리 서비스_ 계층으로 세분화됩니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-106">In the [lambda architecture](../concepts/big-data.md#lambda-architecture), the serving layer is subdivided into a _speed serving_ layer, which stores data that has been processed incrementally, and a _batch serving_ layer, which contains the batch-processed output.</span></span> <span data-ttu-id="0fcbb-107">서비스 계층에서는 짧은 대기 시간의 임의 읽기를 반드시 지원해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-107">The serving layer requires strong support for random reads with low latency.</span></span> <span data-ttu-id="0fcbb-108">또한 빠른 계층용 데이터 저장소는 임의 쓰기도 지원해야 합니다. 이 저장소로 데이터를 일괄 로드하면 원치 않는 지연이 발생하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-108">Data storage for the speed layer should also support random writes, because batch loading data into this store would introduce undesired delays.</span></span> <span data-ttu-id="0fcbb-109">그렇지만 일괄 처리 계층용 데이터 저장소는 임의 쓰기를 지원하지 않아도 되며, 대신 일괄 쓰기를 지원해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-109">On the other hand, data storage for the batch layer does not need to support random writes, but batch writes instead.</span></span>

<span data-ttu-id="0fcbb-110">모든 데이터 저장소 태스크에 가장 적합한 단일 데이터 관리 옵션은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-110">There is no single best data management choice for all data storage tasks.</span></span> <span data-ttu-id="0fcbb-111">태스크마다 최적화된 데이터 관리 솔루션이 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-111">Different data management solutions are optimized for different tasks.</span></span> <span data-ttu-id="0fcbb-112">대부분의 실제 클라우드 앱 및 빅 데이터 프로세스는 다양한 데이터 저장소 요구를 가지며, 데이터 저장소 솔루션을 조합해서 사용하는 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-112">Most real-world cloud apps and big data processes have a variety of data storage requirements and often use a combination of data storage solutions.</span></span>

## <a name="what-are-your-options-when-choosing-an-analytical-data-store"></a><span data-ttu-id="0fcbb-113">분석 데이터 저장소를 선택할 때의 옵션은 무엇인가요?</span><span class="sxs-lookup"><span data-stu-id="0fcbb-113">What are your options when choosing an analytical data store?</span></span>

<span data-ttu-id="0fcbb-114">Azure에서는 사용자의 요구에 따라 다음과 같은 몇 가지 데이터 서비스 저장소 옵션을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-114">There are several options for data serving storage in Azure, depending on your needs:</span></span>

- [<span data-ttu-id="0fcbb-115">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0fcbb-115">SQL Data Warehouse</span></span>](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [<span data-ttu-id="0fcbb-116">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0fcbb-116">Azure SQL Database</span></span>](/azure/sql-database/)
- [<span data-ttu-id="0fcbb-117">Azure VM의 SQL Server</span><span class="sxs-lookup"><span data-stu-id="0fcbb-117">SQL Server in Azure VM</span></span>](/sql/sql-server/sql-server-technical-documentation)
- [<span data-ttu-id="0fcbb-118">HDInsight의 HBase/Phoenix</span><span class="sxs-lookup"><span data-stu-id="0fcbb-118">HBase/Phoenix on HDInsight</span></span>](/azure/hdinsight/hbase/apache-hbase-overview)
- [<span data-ttu-id="0fcbb-119">HDInsight의 Hive LLAP</span><span class="sxs-lookup"><span data-stu-id="0fcbb-119">Hive LLAP on HDInsight</span></span>](/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
- [<span data-ttu-id="0fcbb-120">Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="0fcbb-120">Azure Analysis Services</span></span>](/azure/analysis-services/analysis-services-overview)
- [<span data-ttu-id="0fcbb-121">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0fcbb-121">Azure Cosmos DB</span></span>](/azure/cosmos-db/)

<span data-ttu-id="0fcbb-122">이러한 옵션에서는 다양한 유형의 태스크에 최적화된 다양한 데이터베이스 모델을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-122">These options provide various database models that are optimized for different types of tasks:</span></span>

- <span data-ttu-id="0fcbb-123">[키/값](https://msdn.microsoft.com/library/dn313285.aspx#sec7) 데이터베이스는 각 키 값에 대해 직렬화된 단일 개체를 보유합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-123">[Key/value](https://msdn.microsoft.com/library/dn313285.aspx#sec7) databases hold a single serialized object for each key value.</span></span> <span data-ttu-id="0fcbb-124">이 데이터베이스는 지정된 키 값에 대해 하나의 항목을 가져오려고 하며 항목의 다른 속성을 기준으로 쿼리할 필요가 없는 경우에 대용량의 데이터를 저장하는 데 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-124">They're good for storing large volumes of data where you want to get one item for a given key value and you don't have to query based on other properties of the item.</span></span>
- <span data-ttu-id="0fcbb-125">[문서](https://msdn.microsoft.com/library/dn313285.aspx#sec8) 데이터베이스는 값이 *document*인 키/값 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-125">[Document](https://msdn.microsoft.com/library/dn313285.aspx#sec8) databases are key/value databases in which the values are *documents*.</span></span> <span data-ttu-id="0fcbb-126">이 컨텍스트에서 "document"는 명명된 필드 및 값의 컬렉션입니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-126">A "document" in this context is a collection of named fields and values.</span></span> <span data-ttu-id="0fcbb-127">이 데이터베이스는 일반적으로 XML, YAML, JSON 또는 BSON과 같은 형식으로 데이터를 저장하지만 일반 텍스트를 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-127">The database typically stores the data in a format such as XML, YAML, JSON, or BSON, but may use plain text.</span></span> <span data-ttu-id="0fcbb-128">문서 데이터베이스는 키가 아닌 필드에서 쿼리를 수행하고, 보조 인덱스를 정의하여 쿼리를 보다 효율적으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-128">Document databases can query on non-key fields and define secondary indexes to make querying more efficient.</span></span> <span data-ttu-id="0fcbb-129">따라서 문서 데이터베이스는 문서 키의 값보다 좀 더 복잡한 조건에 따라 데이터를 검색해야 하는 응용 프로그램에 더 적합합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-129">This makes a document database more suitable for applications that need to retrieve data based on criteria more complex than the value of the document key.</span></span> <span data-ttu-id="0fcbb-130">예를 들어, 제품 ID, 고객 ID 또는 고객 이름과 같은 필드에서 쿼리를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-130">For example, you could query on fields such as product ID, customer ID, or customer name.</span></span>
- <span data-ttu-id="0fcbb-131">[열 패밀리](https://msdn.microsoft.com/library/dn313285.aspx#sec9) 데이터베이스는 데이터 저장소를 열 패밀리라고 하는 관련된 열 컬렉션으로 구성하는 키/값 데이터 저장소입니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-131">[Column-family](https://msdn.microsoft.com/library/dn313285.aspx#sec9) databases are key/value data stores that structure data storage into collections of related columns called column families.</span></span> <span data-ttu-id="0fcbb-132">예를 들어, 인구 조사 데이터베이스에는 개인의 이름(이름, 중간 이름, 성)에 대한 열 그룹 1개, 개인의 주소에 대한 그룹 1개, 개인 프로필 정보(생년월일, 성별)에 대한 그룹 1개가 포함될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-132">For example, a census database might have one group of columns for a person's name (first, middle, last), one group for the person's address, and one group for the person's profile information (data of birth, gender).</span></span> <span data-ttu-id="0fcbb-133">이 데이터베이스는 한 사용자의 모든 데이터를 동일한 키와 연결해서 유지하면서 각 열 패밀리를 별도 파티션에 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-133">The database can store each column family in a separate partition, while keeping all of the data for one person related to the same key.</span></span> <span data-ttu-id="0fcbb-134">응용 프로그램은 엔터티의 모든 데이터를 읽지 않더라도 단일 열 패밀리를 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-134">An application can read a single column family without reading through all of the data for an entity.</span></span>
- <span data-ttu-id="0fcbb-135">[그래프](https://msdn.microsoft.com/library/dn313285.aspx#sec10) 데이터베이스는 정보를 개체 및 관계의 컬렉션으로 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-135">[Graph](https://msdn.microsoft.com/library/dn313285.aspx#sec10) databases store information as a collection of objects and relationships.</span></span> <span data-ttu-id="0fcbb-136">그래프 데이터베이스는 개체 네트워크 및 개체 간 관계를 트래버스 통과하는 쿼리를 효율적으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-136">A graph database can efficiently perform queries that traverse the network of objects and the relationships between them.</span></span> <span data-ttu-id="0fcbb-137">예를 들어, 개체가 인사 데이터베이스의 직원일 수 있으며, “find all employees who directly or indirectly work for Scott”와 같은 쿼리를 용이하게 진행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-137">For example, the objects might be employees in a human resources database, and you might want to facilitate queries such as "find all employees who directly or indirectly work for Scott."</span></span>

## <a name="key-selection-criteria"></a><span data-ttu-id="0fcbb-138">주요 선택 조건</span><span class="sxs-lookup"><span data-stu-id="0fcbb-138">Key selection criteria</span></span>

<span data-ttu-id="0fcbb-139">선택 옵션의 범위를 좁히려면 먼저 다음 질문에 답변합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-139">To narrow the choices, start by answering these questions:</span></span>

- <span data-ttu-id="0fcbb-140">데이터에 대해 실행 부하 과다 경로로 사용될 수 있는 저장소를 제공해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="0fcbb-140">Do you need serving storage that can serve as a hot path for your data?</span></span> <span data-ttu-id="0fcbb-141">그렇다면 빠른 서비스 계층에 최적화된 옵션으로 선택 범위를 좁혀보세요.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-141">If yes, narrow your options to those that are optimized for a speed serving layer.</span></span>

- <span data-ttu-id="0fcbb-142">쿼리가 여러 프로세스 또는 노드 사이에서 자동으로 분산되는 MPP(Massively Parallel Processing) 지원이 필요한가요?</span><span class="sxs-lookup"><span data-stu-id="0fcbb-142">Do you need massively parallel processing (MPP) support, where queries are automatically distributed across several processes or nodes?</span></span> <span data-ttu-id="0fcbb-143">그렇다면 쿼리 확장을 지원하는 옵션을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-143">If yes, select an option that supports query scale out.</span></span>

- <span data-ttu-id="0fcbb-144">관계형 데이터 저장소를 사용하고 싶나요?</span><span class="sxs-lookup"><span data-stu-id="0fcbb-144">Do you prefer to use a relational data store?</span></span> <span data-ttu-id="0fcbb-145">그렇다면 관계형 데이터베이스 모델을 포함하는 옵션으로 선택 범위를 좁혀보세요.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-145">If so, narrow your options to those with a relational database model.</span></span> <span data-ttu-id="0fcbb-146">그러나 일부 비관계형 저장소는 쿼리에 대해 SQL 구문을 지원하며, PolyBase와 같은 도구를 사용하여 비관계형 데이터 저장소를 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-146">However, note that some non-relational stores support SQL syntax for querying, and tools such as PolyBase can be used to query non-relational data stores.</span></span>

## <a name="capability-matrix"></a><span data-ttu-id="0fcbb-147">기능 매트릭스</span><span class="sxs-lookup"><span data-stu-id="0fcbb-147">Capability matrix</span></span>

<span data-ttu-id="0fcbb-148">다음 표에서는 주요 기능 차이점을 요약해서 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-148">The following tables summarize the key differences in capabilities.</span></span>

### <a name="general-capabilities"></a><span data-ttu-id="0fcbb-149">일반 기능</span><span class="sxs-lookup"><span data-stu-id="0fcbb-149">General capabilities</span></span>

| | <span data-ttu-id="0fcbb-150">SQL Database</span><span class="sxs-lookup"><span data-stu-id="0fcbb-150">SQL Database</span></span> | <span data-ttu-id="0fcbb-151">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0fcbb-151">SQL Data Warehouse</span></span> | <span data-ttu-id="0fcbb-152">HDInsight의 HBase/Phoenix</span><span class="sxs-lookup"><span data-stu-id="0fcbb-152">HBase/Phoenix on HDInsight</span></span> | <span data-ttu-id="0fcbb-153">HDInsight의 Hive LLAP</span><span class="sxs-lookup"><span data-stu-id="0fcbb-153">Hive LLAP on HDInsight</span></span> | <span data-ttu-id="0fcbb-154">Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="0fcbb-154">Azure Analysis Services</span></span> | <span data-ttu-id="0fcbb-155">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0fcbb-155">Cosmos DB</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0fcbb-156">관리되는 서비스인지 여부</span><span class="sxs-lookup"><span data-stu-id="0fcbb-156">Is managed service</span></span> | <span data-ttu-id="0fcbb-157">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-157">Yes</span></span> | <span data-ttu-id="0fcbb-158">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-158">Yes</span></span> | <span data-ttu-id="0fcbb-159">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-159">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-160">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-160">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-161">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-161">Yes</span></span> | <span data-ttu-id="0fcbb-162">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-162">Yes</span></span> |
| <span data-ttu-id="0fcbb-163">주 데이터베이스 모델</span><span class="sxs-lookup"><span data-stu-id="0fcbb-163">Primary database model</span></span> | <span data-ttu-id="0fcbb-164">관계형(columnstore 인덱스를 사용할 경우 칼럼 형식)</span><span class="sxs-lookup"><span data-stu-id="0fcbb-164">Relational (columnar format when using columnstore indexes)</span></span> | <span data-ttu-id="0fcbb-165">칼럼 형식 저장소가 있는 관계형 테이블</span><span class="sxs-lookup"><span data-stu-id="0fcbb-165">Relational tables with columnar storage</span></span> | <span data-ttu-id="0fcbb-166">넓은 열 저장소</span><span class="sxs-lookup"><span data-stu-id="0fcbb-166">Wide column store</span></span> | <span data-ttu-id="0fcbb-167">Hive/메모리 내</span><span class="sxs-lookup"><span data-stu-id="0fcbb-167">Hive/In-Memory</span></span> | <span data-ttu-id="0fcbb-168">테이블 형식/MOLAP 의미 체계 모델</span><span class="sxs-lookup"><span data-stu-id="0fcbb-168">Tabular/MOLAP semantic models</span></span> | <span data-ttu-id="0fcbb-169">문서 저장소, 그래프, 키-값 저장소, 넓은 열 저장소</span><span class="sxs-lookup"><span data-stu-id="0fcbb-169">Document store, graph, key-value store, wide column store</span></span> |
| <span data-ttu-id="0fcbb-170">SQL 언어 지원</span><span class="sxs-lookup"><span data-stu-id="0fcbb-170">SQL language support</span></span> | <span data-ttu-id="0fcbb-171">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-171">Yes</span></span> | <span data-ttu-id="0fcbb-172">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-172">Yes</span></span> | <span data-ttu-id="0fcbb-173">예([Phoenix](http://phoenix.apache.org/) JDBC 드라이버 사용)</span><span class="sxs-lookup"><span data-stu-id="0fcbb-173">Yes (using [Phoenix](http://phoenix.apache.org/) JDBC driver)</span></span> | <span data-ttu-id="0fcbb-174">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-174">Yes</span></span> | <span data-ttu-id="0fcbb-175">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-175">No</span></span> | <span data-ttu-id="0fcbb-176">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-176">Yes</span></span> |
| <span data-ttu-id="0fcbb-177">빠른 서비스 계층에 최적화됨</span><span class="sxs-lookup"><span data-stu-id="0fcbb-177">Optimized for speed serving layer</span></span> | <span data-ttu-id="0fcbb-178">예 <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-178">Yes <sup>2</sup></span></span> | <span data-ttu-id="0fcbb-179">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-179">No</span></span> | <span data-ttu-id="0fcbb-180">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-180">Yes</span></span> | <span data-ttu-id="0fcbb-181">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-181">Yes</span></span> | <span data-ttu-id="0fcbb-182">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-182">No</span></span> | <span data-ttu-id="0fcbb-183">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-183">Yes</span></span> |

<span data-ttu-id="0fcbb-184">[1] 수동 구성 및 크기 조정 사용</span><span class="sxs-lookup"><span data-stu-id="0fcbb-184">[1] With manual configuration and scaling.</span></span>

<span data-ttu-id="0fcbb-185">[2] 메모리 최적화 테이블 및 해시 또는 비클러스터형 인덱스 사용</span><span class="sxs-lookup"><span data-stu-id="0fcbb-185">[2] Using memory-optimized tables and hash or nonclustered indexes.</span></span>
 
### <a name="scalability-capabilities"></a><span data-ttu-id="0fcbb-186">확장성 기능</span><span class="sxs-lookup"><span data-stu-id="0fcbb-186">Scalability capabilities</span></span>

| | <span data-ttu-id="0fcbb-187">SQL Database</span><span class="sxs-lookup"><span data-stu-id="0fcbb-187">SQL Database</span></span> | <span data-ttu-id="0fcbb-188">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0fcbb-188">SQL Data Warehouse</span></span> | <span data-ttu-id="0fcbb-189">HDInsight의 HBase/Phoenix</span><span class="sxs-lookup"><span data-stu-id="0fcbb-189">HBase/Phoenix on HDInsight</span></span> | <span data-ttu-id="0fcbb-190">HDInsight의 Hive LLAP</span><span class="sxs-lookup"><span data-stu-id="0fcbb-190">Hive LLAP on HDInsight</span></span> | <span data-ttu-id="0fcbb-191">Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="0fcbb-191">Azure Analysis Services</span></span> | <span data-ttu-id="0fcbb-192">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0fcbb-192">Cosmos DB</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0fcbb-193">고가용성을 위한 중복 지역 서버</span><span class="sxs-lookup"><span data-stu-id="0fcbb-193">Redundant regional servers for high availability</span></span>  | <span data-ttu-id="0fcbb-194">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-194">Yes</span></span> | <span data-ttu-id="0fcbb-195">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-195">Yes</span></span> | <span data-ttu-id="0fcbb-196">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-196">Yes</span></span> | <span data-ttu-id="0fcbb-197">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-197">No</span></span> | <span data-ttu-id="0fcbb-198">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-198">No</span></span> | <span data-ttu-id="0fcbb-199">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-199">Yes</span></span> | <span data-ttu-id="0fcbb-200">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-200">Yes</span></span> |
| <span data-ttu-id="0fcbb-201">쿼리 확장 지원 여부</span><span class="sxs-lookup"><span data-stu-id="0fcbb-201">Supports query scale out</span></span>  | <span data-ttu-id="0fcbb-202">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-202">No</span></span> | <span data-ttu-id="0fcbb-203">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-203">Yes</span></span> | <span data-ttu-id="0fcbb-204">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-204">Yes</span></span> | <span data-ttu-id="0fcbb-205">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-205">Yes</span></span> | <span data-ttu-id="0fcbb-206">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-206">Yes</span></span> | <span data-ttu-id="0fcbb-207">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-207">Yes</span></span> |
| <span data-ttu-id="0fcbb-208">동적 확장성(강화)</span><span class="sxs-lookup"><span data-stu-id="0fcbb-208">Dynamic scalability (scale up)</span></span>  | <span data-ttu-id="0fcbb-209">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-209">Yes</span></span> | <span data-ttu-id="0fcbb-210">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-210">Yes</span></span> | <span data-ttu-id="0fcbb-211">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-211">No</span></span> | <span data-ttu-id="0fcbb-212">아니오</span><span class="sxs-lookup"><span data-stu-id="0fcbb-212">No</span></span> | <span data-ttu-id="0fcbb-213">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-213">Yes</span></span> | <span data-ttu-id="0fcbb-214">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-214">Yes</span></span> |
| <span data-ttu-id="0fcbb-215">데이터의 메모리 내 캐싱 지원 여부</span><span class="sxs-lookup"><span data-stu-id="0fcbb-215">Supports in-memory caching of data</span></span> | <span data-ttu-id="0fcbb-216">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-216">Yes</span></span> | <span data-ttu-id="0fcbb-217">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-217">Yes</span></span> | <span data-ttu-id="0fcbb-218">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-218">No</span></span> | <span data-ttu-id="0fcbb-219">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-219">Yes</span></span> | <span data-ttu-id="0fcbb-220">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-220">Yes</span></span> | <span data-ttu-id="0fcbb-221">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-221">No</span></span> |

### <a name="security-capabilities"></a><span data-ttu-id="0fcbb-222">보안 기능</span><span class="sxs-lookup"><span data-stu-id="0fcbb-222">Security capabilities</span></span>

| | <span data-ttu-id="0fcbb-223">SQL Database</span><span class="sxs-lookup"><span data-stu-id="0fcbb-223">SQL Database</span></span> | <span data-ttu-id="0fcbb-224">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0fcbb-224">SQL Data Warehouse</span></span> | <span data-ttu-id="0fcbb-225">HDInsight의 HBase/Phoenix</span><span class="sxs-lookup"><span data-stu-id="0fcbb-225">HBase/Phoenix on HDInsight</span></span> | <span data-ttu-id="0fcbb-226">HDInsight의 Hive LLAP</span><span class="sxs-lookup"><span data-stu-id="0fcbb-226">Hive LLAP on HDInsight</span></span> | <span data-ttu-id="0fcbb-227">Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="0fcbb-227">Azure Analysis Services</span></span> | <span data-ttu-id="0fcbb-228">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0fcbb-228">Cosmos DB</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0fcbb-229">인증</span><span class="sxs-lookup"><span data-stu-id="0fcbb-229">Authentication</span></span>  | <span data-ttu-id="0fcbb-230">SQL/Azure AD(Azure Active Directory)</span><span class="sxs-lookup"><span data-stu-id="0fcbb-230">SQL / Azure Active Directory (Azure AD)</span></span> | <span data-ttu-id="0fcbb-231">SQL/Azure AD</span><span class="sxs-lookup"><span data-stu-id="0fcbb-231">SQL / Azure AD</span></span> | <span data-ttu-id="0fcbb-232">로컬/Azure AD <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-232">local / Azure AD <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-233">로컬/Azure AD <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-233">local / Azure AD <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-234">Azure AD</span><span class="sxs-lookup"><span data-stu-id="0fcbb-234">Azure AD</span></span> | <span data-ttu-id="0fcbb-235">액세스 제어(IAM)을 통한 데이터베이스 사용자/Azure AD</span><span class="sxs-lookup"><span data-stu-id="0fcbb-235">database users / Azure AD via access control (IAM)</span></span> |
| <span data-ttu-id="0fcbb-236">휴지 상태의 암호화</span><span class="sxs-lookup"><span data-stu-id="0fcbb-236">Data encryption at rest</span></span> | <span data-ttu-id="0fcbb-237">예 <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-237">Yes <sup>2</sup></span></span> | <span data-ttu-id="0fcbb-238">예 <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-238">Yes <sup>2</sup></span></span> | <span data-ttu-id="0fcbb-239">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-239">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-240">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-240">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-241">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-241">Yes</span></span> | <span data-ttu-id="0fcbb-242">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-242">Yes</span></span> |
| <span data-ttu-id="0fcbb-243">행 수준 보안</span><span class="sxs-lookup"><span data-stu-id="0fcbb-243">Row-level security</span></span> | <span data-ttu-id="0fcbb-244">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-244">Yes</span></span> | <span data-ttu-id="0fcbb-245">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-245">No</span></span> | <span data-ttu-id="0fcbb-246">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-246">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-247">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-247">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-248">예(모델에 개체 수준 보안 사용)</span><span class="sxs-lookup"><span data-stu-id="0fcbb-248">Yes (through object-level security in model)</span></span> | <span data-ttu-id="0fcbb-249">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-249">No</span></span> |
| <span data-ttu-id="0fcbb-250">방화벽 지원 여부</span><span class="sxs-lookup"><span data-stu-id="0fcbb-250">Supports firewalls</span></span> | <span data-ttu-id="0fcbb-251">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-251">Yes</span></span> | <span data-ttu-id="0fcbb-252">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-252">Yes</span></span> | <span data-ttu-id="0fcbb-253">예 <sup>3</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-253">Yes <sup>3</sup></span></span> | <span data-ttu-id="0fcbb-254">예 <sup>3</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-254">Yes <sup>3</sup></span></span> | <span data-ttu-id="0fcbb-255">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-255">Yes</span></span> | <span data-ttu-id="0fcbb-256">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-256">Yes</span></span> |
| <span data-ttu-id="0fcbb-257">동적 데이터 마스킹</span><span class="sxs-lookup"><span data-stu-id="0fcbb-257">Dynamic data masking</span></span> | <span data-ttu-id="0fcbb-258">예</span><span class="sxs-lookup"><span data-stu-id="0fcbb-258">Yes</span></span> | <span data-ttu-id="0fcbb-259">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-259">No</span></span> | <span data-ttu-id="0fcbb-260">예 <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="0fcbb-260">Yes <sup>1</sup></span></span> | <span data-ttu-id="0fcbb-261">예 \*</span><span class="sxs-lookup"><span data-stu-id="0fcbb-261">Yes \*</span></span> | <span data-ttu-id="0fcbb-262">아니오</span><span class="sxs-lookup"><span data-stu-id="0fcbb-262">No</span></span> | <span data-ttu-id="0fcbb-263">아니요</span><span class="sxs-lookup"><span data-stu-id="0fcbb-263">No</span></span> |

<span data-ttu-id="0fcbb-264">[1] [도메인 가입 HDInsight 클러스터](/azure/hdinsight/domain-joined/apache-domain-joined-introduction)를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-264">[1] Requires using a [domain-joined HDInsight cluster](/azure/hdinsight/domain-joined/apache-domain-joined-introduction).</span></span>

<span data-ttu-id="0fcbb-265">[2] 미사용 데이터의 암호화 및 암호 해독을 위해 TDE(투명한 데이터 암호화)를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-265">[2] Requires using transparent data encryption (TDE) to encrypt and decrypt your data at rest.</span></span>

<span data-ttu-id="0fcbb-266">[3] Azure Virtual Network 내에서 사용할 때.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-266">[3] When used within an Azure Virtual Network.</span></span> <span data-ttu-id="0fcbb-267">[Azure Virtual Network를 사용하여 Azure HDInsight 확장](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="0fcbb-267">See [Extend Azure HDInsight using an Azure Virtual Network](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network).</span></span>