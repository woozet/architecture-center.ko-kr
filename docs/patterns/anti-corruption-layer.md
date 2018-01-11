---
title: "손상 방지 레이어 패턴"
description: "현대식 응용 프로그램과 레거시 시스템 사이에 외관 또는 어댑터 레이어를 구현합니다."
author: dragon119
ms.date: 06/23/2017
ms.openlocfilehash: 590d5f3676c92f5f18661360106e2b2fdd4efbe1
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2017
---
# <a name="anti-corruption-layer-pattern"></a><span data-ttu-id="fa418-103">손상 방지 레이어 패턴</span><span class="sxs-lookup"><span data-stu-id="fa418-103">Anti-Corruption Layer pattern</span></span>

<span data-ttu-id="fa418-104">최신 응용 프로그램과 이 응용 프로그램이 의존하는 레거시 시스템 사이에 외관 또는 어댑터 레이어를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-104">Implement a façade or adapter layer between a modern application and a legacy system that it depends on.</span></span> <span data-ttu-id="fa418-105">이 레이어는 최신 응용 프로그램과 레거시 시스템 간에 요청을 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-105">This layer translates requests between the modern application and the legacy system.</span></span> <span data-ttu-id="fa418-106">이 패턴을 사용하여 응용 프로그램의 디자인이 레거시 시스템에 대한 종속성으로 제한되지 않도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-106">Use this pattern to ensure that an application's design is not limited by dependencies on legacy systems.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="fa418-107">컨텍스트 및 문제점</span><span class="sxs-lookup"><span data-stu-id="fa418-107">Context and problem</span></span>

<span data-ttu-id="fa418-108">대부분의 응용 프로그램은 일부 데이터 또는 기능을 위해 다른 시스템에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-108">Most applications rely on other systems for some data or functionality.</span></span> <span data-ttu-id="fa418-109">예를 들어 레거시 응용 프로그램을 최신 시스템으로 마이그레이션해도 기존 레거시 리소스가 여전히 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-109">For example, when a legacy application is migrated to a modern system, it may still need existing legacy resources.</span></span> <span data-ttu-id="fa418-110">새로운 기능은 레거시 시스템을 호출할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-110">New features must be able to call the legacy system.</span></span> <span data-ttu-id="fa418-111">시간이 지나면서 더 큰 응용 프로그램의 다른 기능을 최신 시스템으로 이동하게 되는 점진적 마이그레이션에서 특히 그렇습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-111">This is especially true of gradual migrations, where different features of a larger application are moved to a modern system over time.</span></span>

<span data-ttu-id="fa418-112">종종 이러한 레거시 시스템에서 복잡한 데이터 스키마 또는 사용되지 않는 API와 같은 품질 문제가 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-112">Often these legacy systems suffer from quality issues such as convoluted data schemas or obsolete APIs.</span></span> <span data-ttu-id="fa418-113">레거시 시스템에서 사용되는 기능 및 기술은 최신 시스템의 경우와 크게 다를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-113">The features and technologies used in legacy systems can vary widely from more modern systems.</span></span> <span data-ttu-id="fa418-114">레거시 시스템과 상호 작용하기 위해 새 응용 프로그램은 오래된 인프라, 프로토콜, 데이터 모델, API 또는 최신 응용 프로그램에 추가되지 않은 기타 기능을 지원해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-114">To interoperate with the legacy system, the new application may need to support outdated infrastructure, protocols, data models, APIs, or other features that you wouldn't otherwise put into a modern application.</span></span>

<span data-ttu-id="fa418-115">새 시스템과 레거시 시스템 간에 액세스를 유지 관리하면 새 시스템이 적어도 레거시 시스템 API의 일부 또는 기타 의미 체계를 강제로 준수할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-115">Maintaining access between new and legacy systems can force the new system to adhere to at least some of the legacy system's APIs or other semantics.</span></span> <span data-ttu-id="fa418-116">품질 문제가 있는 레거시 기능을 지원하면 완전하게 디자인된 최신 응용 프로그램에 “손상”을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-116">When these legacy features have quality issues, supporting them "corrupts" what might otherwise be a cleanly designed modern application.</span></span> 

## <a name="solution"></a><span data-ttu-id="fa418-117">해결 방법</span><span class="sxs-lookup"><span data-stu-id="fa418-117">Solution</span></span>

<span data-ttu-id="fa418-118">손상 방지 레이어를 레거시 시스템과 최신 시스템 사이에 배치하여 두 시스템을 격리합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-118">Isolate the legacy and modern systems by placing an anti-corruption layer between them.</span></span> <span data-ttu-id="fa418-119">이 레이어는 두 시스템 간의 통신을 변환하여, 최신 응용 프로그램이 레거시 시스템의 디자인 및 기술 방식을 손상시키지 않도록 하여 레거시 시스템을 변경되지 않은 상태로 유지하도록 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-119">This layer translates communications between the two systems, allowing the legacy system to remain unchanged while the modern application can avoid compromising its design and technological approach.</span></span>

![](./_images/anti-corruption-layer.png) 

<span data-ttu-id="fa418-120">최신 응용 프로그램과 손상 방지 레이어 간 통신에는 항상 응용 프로그램의 데이터 모델 및 아키텍처가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-120">Communication between the modern application and the anti-corruption layer always uses the application's data model and architecture.</span></span> <span data-ttu-id="fa418-121">손상 방지 레이어에서 레거시 시스템으로의 호출은 해당 시스템의 데이터 모델 또는 메서드를 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-121">Calls from the anti-corruption layer to the legacy system conform to that system's data model or methods.</span></span> <span data-ttu-id="fa418-122">손상 방지 레이어는 두 시스템 사이의 변환에 필요한 모든 논리를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-122">The anti-corruption layer contains all of the logic necessary to translate between the two systems.</span></span> <span data-ttu-id="fa418-123">이러한 레이어는 응용 프로그램 내의 구성 요소로 또는 독립적인 서비스로 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-123">The layer can be implemented as a component within the application or as an independent service.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="fa418-124">문제 및 고려 사항</span><span class="sxs-lookup"><span data-stu-id="fa418-124">Issues and considerations</span></span>

- <span data-ttu-id="fa418-125">손상 방지 레이어가 있는 경우 두 시스템 간에 수행되는 호출이 지연될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-125">The anti-corruption layer may add latency to calls made between the two systems.</span></span>
- <span data-ttu-id="fa418-126">손상 방지 레이어는 관리 및 유지해야 하는 추가 서비스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-126">The anti-corruption layer adds an additional service that must be managed and maintained.</span></span>
- <span data-ttu-id="fa418-127">손상 방지 레이어의 크기를 조정하는 방식을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-127">Consider how your anti-corruption layer will scale.</span></span>
- <span data-ttu-id="fa418-128">둘 이상의 손상 방지 레이어가 필요한지 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-128">Consider whether you need more than one anti-corruption layer.</span></span> <span data-ttu-id="fa418-129">다양한 기술 또는 언어를 사용하여 기능을 여러 서비스로 분해하려고 하거나, 손상 방지 레이어를 분할할 다른 이유가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-129">You may want to decompose functionality into multiple services using different technologies or languages, or there may be other reasons to partition the anti-corruption layer.</span></span>
- <span data-ttu-id="fa418-130">다른 응용 프로그램 또는 서비스와의 관계를 고려해서 손상 방지 레이어를 관리하는 방법을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-130">Consider how the anti-corruption layer will be managed in relation with your other applications or services.</span></span> <span data-ttu-id="fa418-131">손상 방지 레이어가 모니터링, 릴리스 및 구성 프로세스에 어떻게 통합될 수 있을까요?</span><span class="sxs-lookup"><span data-stu-id="fa418-131">How will it be integrated into your monitoring, release, and configuration processes?</span></span>
- <span data-ttu-id="fa418-132">트랜잭션 및 데이터 일관성이 유지 관리되고 모니터링할 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-132">Make sure transaction and data consistency are maintained and can be monitored.</span></span>
- <span data-ttu-id="fa418-133">손상 방지 레이어가 레거시 및 최신 시스템 간의 모든 통신을 처리해야 하는지 또는 기능 하위 집합만 처리하면 되는지를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-133">Consider whether the anti-corruption layer needs to handle all communication between legacy and modern systems, or just a subset of features.</span></span> 
- <span data-ttu-id="fa418-134">손상 방지 레이어가 영구적으로 사용될지 또는 모든 레거시 기능이 마이그레이션된 후에 더 이상 사용되지 않을지를 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-134">Consider whether the anti-corruption layer is meant to be permanent, or eventually retired once all legacy functionality has been migrated.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="fa418-135">이 패턴을 사용해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fa418-135">When to use this pattern</span></span>

<span data-ttu-id="fa418-136">다음의 경우에 이 패턴을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-136">Use this pattern when:</span></span>

- <span data-ttu-id="fa418-137">마이그레이션이 여러 단계를 통해 진행되도록 계획되어 있지만 새 시스템과 레거시시 스템 간의 통합을 유지 관리해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fa418-137">A migration is planned to happen over multiple stages, but integration between new and legacy systems needs to be maintained.</span></span>
- <span data-ttu-id="fa418-138">새 시스템과 레거시 시스템이 서로 다른 의미 체계를 갖지만 여전히 통신해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="fa418-138">New and legacy system have different semantics, but still need to communicate.</span></span>

<span data-ttu-id="fa418-139">새 시스템과 레거시 시스템 간의 큰 의미 체계 차이가 없는 경우에는 이 패턴이 적합하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa418-139">This pattern may not be suitable if there are no significant semantic differences between new and legacy systems.</span></span> 

## <a name="related-guidance"></a><span data-ttu-id="fa418-140">관련 지침</span><span class="sxs-lookup"><span data-stu-id="fa418-140">Related guidance</span></span>

- <span data-ttu-id="fa418-141">[스트랭글러 패턴][strangler]</span><span class="sxs-lookup"><span data-stu-id="fa418-141">[Strangler pattern][strangler]</span></span>

[strangler]: ./strangler.md