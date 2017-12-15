---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: "効率的なデータのページングを実装して |Microsoft ドキュメント"
author: microsoft
description: "手順 8 では、一度にディナーの 1,000 件を表示するには、代わりにのみ表示で 10 今後ディナーように、/Dinners URL にページング サポートを追加する方法を示します."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0b0fba604f97d3bb72d2d403e643b422b9ce48bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="implement-efficient-data-paging"></a><span data-ttu-id="30dc8-103">効率的なデータのページングを実装します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-103">Implement Efficient Data Paging</span></span>
====================
<span data-ttu-id="30dc8-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="30dc8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="30dc8-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="30dc8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="30dc8-106">これは、無料の手順 8 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="30dc8-106">This is step 8 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="30dc8-107">手順 8 では、ディナー一度にの 1,000 件を表示するには、代わりにうまくのみ一度に - 10 今後ディナーを表示し、へ戻るページし、SEO フレンドリな方法ですべてのリストを転送するエンドユーザーを許可するように、/Dinners URL にページング サポートを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-107">Step 8 shows how to add paging support to our /Dinners URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>
> 
> <span data-ttu-id="30dc8-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="30dc8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-8-paging-support"></a><span data-ttu-id="30dc8-109">NerdDinner 手順 8: ページングのサポート</span><span class="sxs-lookup"><span data-stu-id="30dc8-109">NerdDinner Step 8: Paging Support</span></span>

<span data-ttu-id="30dc8-110">サイトが成功した場合は、今後ディナー数千になります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-110">If our site is successful, it will have thousands of upcoming dinners.</span></span> <span data-ttu-id="30dc8-111">UI がすべてこれらディナーの処理を拡張し、により、ユーザーはそれらを参照することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-111">We need to make sure that our UI scales to handle all of these dinners, and allows users to browse them.</span></span> <span data-ttu-id="30dc8-112">ページング サポートを追加すると、これを有効にするには、当社*/Dinners*のみありますに 1 回、おディナーの 1,000 件を表示する方法の代わりに URL を一度に - 10 今後ディナーを表示し、エンドユーザーがページの背面とのすべてのリストを転送するを許可します。SEO フレンドリな方法です。</span><span class="sxs-lookup"><span data-stu-id="30dc8-112">To enable this, we'll add paging support to our */Dinners* URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>

### <a name="index-action-method-recap"></a><span data-ttu-id="30dc8-113">Index() アクション メソッドの要約</span><span class="sxs-lookup"><span data-stu-id="30dc8-113">Index() Action Method Recap</span></span>

<span data-ttu-id="30dc8-114">Index() アクション メソッド、DinnersController クラス内で現在よう下。</span><span class="sxs-lookup"><span data-stu-id="30dc8-114">The Index() action method within our DinnersController class currently looks like below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

<span data-ttu-id="30dc8-115">ときに、要求が行われる、 */Dinners* URL、今後すべてディナーの一覧を取得し、それらのすべての一覧を表示。</span><span class="sxs-lookup"><span data-stu-id="30dc8-115">When a request is made to the */Dinners* URL, it retrieves a list of all upcoming dinners and then renders a listing of all of them out:</span></span>

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a><span data-ttu-id="30dc8-116">Understanding IQuerable&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="30dc8-116">Understanding IQuerable&lt;T&gt;</span></span>

<span data-ttu-id="30dc8-117">*IQueryable&lt;T&gt;* は .NET 3.5 の一部として、LINQ で導入されたインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="30dc8-117">*IQueryable&lt;T&gt;* is an interface that was introduced with LINQ as part of .NET 3.5.</span></span> <span data-ttu-id="30dc8-118">お利用できるにページング サポートを実装する強力な「遅延実行」のシナリオを可能になります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-118">It enables powerful "deferred execution" scenarios that we can take advantage of to implement paging support.</span></span>

<span data-ttu-id="30dc8-119">IQueryable を返すことは、DinnerRepository で&lt;Dinner&gt; FindUpcomingDinners() メソッドからシーケンス。</span><span class="sxs-lookup"><span data-stu-id="30dc8-119">In our DinnerRepository we are returning an IQueryable&lt;Dinner&gt; sequence from our FindUpcomingDinners() method:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

<span data-ttu-id="30dc8-120">IQueryable&lt;Dinner&gt; FindUpcomingDinners() メソッドによって返されるオブジェクトは、LINQ to SQL を使用して、データベースから Dinner オブジェクトを取得するクエリをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-120">The IQueryable&lt;Dinner&gt; object returned by our FindUpcomingDinners() method encapsulates a query to retrieve Dinner objects from our database using LINQ to SQL.</span></span> <span data-ttu-id="30dc8-121">重要なは、クエリ内のデータに対するアクセス/反復処理するまで、またはそれに ToList() メソッドを呼び出してデータベースに対してクエリを実行ことはありません。</span><span class="sxs-lookup"><span data-stu-id="30dc8-121">Importantly, it won't execute the query against the database until we attempt to access/iterate over the data in the query, or until we call the ToList() method on it.</span></span> <span data-ttu-id="30dc8-122">当社 FindUpcomingDinners() メソッドを呼び出すコードは、必要に応じてフィルターを追加するその他「チェーン」操作/IQueryable に選択できます&lt;Dinner&gt;オブジェクト クエリを実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="30dc8-122">The code calling our FindUpcomingDinners() method can optionally choose to add additional "chained" operations/filters to the IQueryable&lt;Dinner&gt; object before executing the query.</span></span> <span data-ttu-id="30dc8-123">LINQ to SQL はデータが要求されたときに、結合されたデータベースに対してクエリを実行するのに十分なスマートになります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-123">LINQ to SQL is then smart enough to execute the combined query against the database when the data is requested.</span></span>

<span data-ttu-id="30dc8-124">ページングのロジックを実装する更新することが、DinnersController の Index() アクション メソッドに返される IQueryable"Skip"と「実行」の追加の演算子が適用されるよう&lt;Dinner&gt;で ToList() を呼び出す前にシーケンス。</span><span class="sxs-lookup"><span data-stu-id="30dc8-124">To implement paging logic we can update our DinnersController's Index() action method so that it applies additional "Skip" and "Take" operators to the returned IQueryable&lt;Dinner&gt; sequence before calling ToList() on it:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

<span data-ttu-id="30dc8-125">上記のコードでは、データベース内の最初の 10 個の今後ディナーをスキップし、バックアップ 20 ディナーを返します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-125">The above code skips over the first 10 upcoming dinners in the database, and then returns back 20 dinners.</span></span> <span data-ttu-id="30dc8-126">LINQ to SQL は – SQL データベースと web サーバーではなくロジックをスキップしています。 これを実行する最適化された SQL クエリを構築できるほどです。</span><span class="sxs-lookup"><span data-stu-id="30dc8-126">LINQ to SQL is smart enough to construct an optimized SQL query that performs this skipping logic in the SQL database – and not in the web-server.</span></span> <span data-ttu-id="30dc8-127">これは、何百万も今後ディナーのデータベースにある、場合でも、必要な 10 だけは (やすく効率的でスケーラブルな) この要求の一部として取得することを意味します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-127">This means that even if we have millions of upcoming Dinners in the database, only the 10 we want will be retrieved as part of this request (making it efficient and scalable).</span></span>

### <a name="adding-a-page-value-to-the-url"></a><span data-ttu-id="30dc8-128">URL に「ページ」の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-128">Adding a "page" value to the URL</span></span>

<span data-ttu-id="30dc8-129">ハードコーディングする代わりに、特定のページ範囲、ユーザーが要求しているどの Dinner 範囲を示す「ページ」のパラメーターを含む Url がいいでしょう。</span><span class="sxs-lookup"><span data-stu-id="30dc8-129">Instead of hard-coding a specific page range, we'll want our URLs to include a "page" parameter that indicates which Dinner range a user is requesting.</span></span>

#### <a name="using-a-querystring-value"></a><span data-ttu-id="30dc8-130">クエリ文字列値を使用します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-130">Using a Querystring value</span></span>

<span data-ttu-id="30dc8-131">次のコードは、クエリ文字列パラメーターをサポートすると同様に Url を有効にする、Index() アクション メソッドを更新お方法を示します*/Dinners しますか? ページ 2 =*:</span><span class="sxs-lookup"><span data-stu-id="30dc8-131">The code below demonstrates how we can update our Index() action method to support a querystring parameter and enable URLs like */Dinners?page=2*:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

<span data-ttu-id="30dc8-132">上記の Index() アクション メソッドには、「ページ」を名前付きパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-132">The Index() action method above has a parameter named "page".</span></span> <span data-ttu-id="30dc8-133">パラメーターが null 許容の整数として宣言されている (はどのような int? を示します)。</span><span class="sxs-lookup"><span data-stu-id="30dc8-133">The parameter is declared as a nullable integer (that is what int? indicates).</span></span> <span data-ttu-id="30dc8-134">つまり、 */Dinners しますか? ページ 2 =* URL パラメーター値として渡されるには、「2」の値になります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-134">This means that the */Dinners?page=2* URL will cause a value of "2" to be passed as the parameter value.</span></span> <span data-ttu-id="30dc8-135">*/Dinners* URL (クエリ文字列の値) が渡される null 値になります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-135">The */Dinners* URL (without a querystring value) will cause a null value to be passed.</span></span>

<span data-ttu-id="30dc8-136">おはサイズを乗算ページの値 ページ (この場合は 10 行) をスキップする数ディナーを決定します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-136">We are multiplying the page value by the page size (in this case 10 rows) to determine how many dinners to skip over.</span></span> <span data-ttu-id="30dc8-137">使用して、 [c# null「合体」演算子 (?)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)は null 許容型を処理する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="30dc8-137">We are using the [C# null "coalescing" operator (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) which is useful when dealing with nullable types.</span></span> <span data-ttu-id="30dc8-138">上記のコードでは、ページのパラメーターが null の場合、ページが 0 の値に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-138">The code above assigns page the value of 0 if the page parameter is null.</span></span>

#### <a name="using-embedded-url-values"></a><span data-ttu-id="30dc8-139">埋め込まれた URL 値を使用します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-139">Using Embedded URL values</span></span>

<span data-ttu-id="30dc8-140">クエリ文字列の値を使用する代わりには、実際の URL 自体内でページのパラメーターを埋め込むになります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-140">An alternative to using a querystring value would be to embed the page parameter within the actual URL itself.</span></span> <span data-ttu-id="30dc8-141">例: */Dinners/Page/2*または*ディナー/2*です。</span><span class="sxs-lookup"><span data-stu-id="30dc8-141">For example: */Dinners/Page/2* or */Dinners/2*.</span></span> <span data-ttu-id="30dc8-142">ASP.NET MVC には、強力な URL ルーティング エンジン容易に次のようなシナリオをサポートするものにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="30dc8-142">ASP.NET MVC includes a powerful URL routing engine that makes it easy to support scenarios like this.</span></span>

<span data-ttu-id="30dc8-143">カスタムのルーティング規則受信 URL または URL を選びましたコント ローラー クラスまたはアクション メソッドをフォーマットするマップ登録できます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-143">We can register custom routing rules that map any incoming URL or URL format to any controller class or action method we want.</span></span> <span data-ttu-id="30dc8-144">タスクが必要なは、プロジェクト内での Global.asax ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-144">All we need to-do is to open the Global.asax file within our project:</span></span>

![](implement-efficient-data-paging/_static/image2.png)

<span data-ttu-id="30dc8-145">ルートの最初の呼び出しのような MapRoute() ヘルパー メソッドを使用して、新しいマッピング ルールを登録します。MapRoute() 下:</span><span class="sxs-lookup"><span data-stu-id="30dc8-145">And then register a new mapping rule using the MapRoute() helper method like the first call to routes.MapRoute() below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

<span data-ttu-id="30dc8-146">上記の"UpcomingDinners"をという名前の新しいルーティング ルールが登録されます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-146">Above we are registering a new routing rule named "UpcomingDinners".</span></span> <span data-ttu-id="30dc8-147">URL の形式であることを示しているお"ディナー/ページ/{ページ}"– {ページ} は、URL に埋め込まれたパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="30dc8-147">We are indicating it has the URL format "Dinners/Page/{page}" – where {page} is a parameter value embedded within the URL.</span></span> <span data-ttu-id="30dc8-148">MapRoute() メソッドを 3 番目のパラメーターは、DinnersController クラス Index() アクション メソッドには、この形式に一致する Url を割り当てる必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-148">The third parameter to the MapRoute() method indicates that we should map URLs that match this format to the Index() action method on the DinnersController class.</span></span>

<span data-ttu-id="30dc8-149">発生しました。 前に – クエリ文字列に紹介したシナリオと、"page"パラメーターは、URL と、クエリ文字列ではないから取得するようになりました点を除いてとまったく同じ Index() コードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-149">We can use the exact same Index() code we had before with our Querystring scenario – except now our "page" parameter will come from the URL and not the querystring:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

<span data-ttu-id="30dc8-150">今すぐおアプリケーションを実行および入力*/Dinners*最初の 10 個の今後のディナー後ほどお見せします。</span><span class="sxs-lookup"><span data-stu-id="30dc8-150">And now when we run the application and type in */Dinners* we'll see the first 10 upcoming dinners:</span></span>

![](implement-efficient-data-paging/_static/image3.png)

<span data-ttu-id="30dc8-151">ときに入力することと*/Dinners/Page/1*おディナーの次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-151">And when we type in */Dinners/Page/1* we'll see the next page of dinners:</span></span>

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a><span data-ttu-id="30dc8-152">ページ ナビゲーション UI を追加します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-152">Adding page navigation UI</span></span>

<span data-ttu-id="30dc8-153">ページングに紹介したシナリオを完了する最後の手順は、Dinner データを簡単にスキップするユーザーを有効にするには、当社ビュー テンプレート内で、次へ」や「以前」のナビゲーション UI を実装するされます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-153">The last step to complete our paging scenario will be to implement "next" and "previous" navigation UI within our view template to enable users to easily skip over the Dinner data.</span></span>

<span data-ttu-id="30dc8-154">これを正しく実装するには必要がありますディナーの合計数をデータベースにわかっているにも方法と、データの多くのページと解釈されます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-154">To implement this correctly, we'll need to know the total number of Dinners in the database, as well as how many pages of data this translates to.</span></span> <span data-ttu-id="30dc8-155">現在の要求の「ページ」の値を先頭または、データの末尾にあるかどうか計算および表示を切り替えるそれに応じて「以前」と [次へ] の UI を非表示にし必要になります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-155">We'll then need to calculate whether the currently requested "page" value is at the beginning or end of the data, and show or hide the "previous" and "next" UI accordingly.</span></span> <span data-ttu-id="30dc8-156">このロジックを実装すると、当社 Index() アクション メソッド内でお可能性があります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-156">We could implement this logic within our Index() action method.</span></span> <span data-ttu-id="30dc8-157">代わりにヘルパー クラスを再利用可能な複数の方法でこのロジックをカプセル化するプロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-157">Alternatively we can add a helper class to our project that encapsulates this logic in a more re-usable way.</span></span>

<span data-ttu-id="30dc8-158">一覧から派生した単純な"PaginatedList"ヘルパー クラスを次に示します&lt;T&gt;に組み込まコレクション クラス、.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="30dc8-158">Below is a simple "PaginatedList" helper class that derives from the List&lt;T&gt; collection class built-into the .NET Framework.</span></span> <span data-ttu-id="30dc8-159">IQueryable データの任意のシーケンスを改ページ調整に使用できる再利用可能なコレクション クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-159">It implements a re-usable collection class that can be used to paginate any sequence of IQueryable data.</span></span> <span data-ttu-id="30dc8-160">NerdDinner アプリケーションでは必要がある IQueryable 経由で動作する&lt;Dinner&gt;結果が同じくらい簡単にでも使用 IQueryable&lt;製品&gt;または IQueryable&lt;顧客&gt;他のアプリケーション シナリオで発生します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-160">In our NerdDinner application we'll have it work over IQueryable&lt;Dinner&gt; results, but it could just as easily be used against IQueryable&lt;Product&gt; or IQueryable&lt;Customer&gt; results in other application scenarios:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

<span data-ttu-id="30dc8-161">上記を計算する方法および"PageIndex"、"PageSize"、"TotalCount"および"TotalPages"プロパティを公開と同様に、されます。</span><span class="sxs-lookup"><span data-stu-id="30dc8-161">Notice above how it calculates and then exposes properties like "PageIndex", "PageSize", "TotalCount", and "TotalPages".</span></span> <span data-ttu-id="30dc8-162">ヘルパーの 2 つのプロパティ"HasPreviousPage"と"HasNextPage"コレクション内のデータのページが先頭または元のシーケンスの末尾にあるかどうかを示すも公開します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-162">It also then exposes two helper properties "HasPreviousPage" and "HasNextPage" that indicate whether the page of data in the collection is at the beginning or end of the original sequence.</span></span> <span data-ttu-id="30dc8-163">上記のコードを実行する - Dinner オブジェクトの合計数のカウントを取得する最初の 2 つの SQL クエリとなります (これは、オブジェクトを返さない – 整数を返す「数の選択」ステートメントの実行ではなく)、2 番目の行だけを取得するにはデータのデータの現在のページをデータベースから必要があります。</span><span class="sxs-lookup"><span data-stu-id="30dc8-163">The above code will cause two SQL queries to be run - the first to retrieve the count of the total number of Dinner objects (this doesn't return the objects – rather it performs a "SELECT COUNT" statement that returns an integer), and the second to retrieve just the rows of data we need from our database for the current page of data.</span></span>

<span data-ttu-id="30dc8-164">当社 DinnersController.Index() ヘルパー メソッド、PaginatedList を作成し、更新することが&lt;Dinner&gt; DinnerRepository.FindUpcomingDinners() から発生し、このビュー テンプレートに渡すこと。</span><span class="sxs-lookup"><span data-stu-id="30dc8-164">We can then update our DinnersController.Index() helper method to create a PaginatedList&lt;Dinner&gt; from our DinnerRepository.FindUpcomingDinners() result, and pass it to our view template:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

<span data-ttu-id="30dc8-165">ViewPage から継承する \Views\Dinners\Index.aspx ビュー テンプレート、更新することが&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; ViewPage ではなく&lt;IEnumerable&lt;Dinner&gt;&gt;、前後のナビゲーション UI を非表示のビュー テンプレートの下部に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-165">We can then update the \Views\Dinners\Index.aspx view template to inherit from ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt;&gt; instead of ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;, and then add the following code to the bottom of our view-template to show or hide next and previous navigation UI:</span></span>

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

<span data-ttu-id="30dc8-166">方法を使用して上記 Html.RouteLink() ヘルパー メソッド、ハイパーリンクを生成することを確認します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-166">Notice above how we are using the Html.RouteLink() helper method to generate our hyperlinks.</span></span> <span data-ttu-id="30dc8-167">このメソッドは、以前に使用した Html.ActionLink() ヘルパー メソッドに似ています。</span><span class="sxs-lookup"><span data-stu-id="30dc8-167">This method is similar to the Html.ActionLink() helper method we've used previously.</span></span> <span data-ttu-id="30dc8-168">違いは、ルール、Global.asax ファイル内でセットアップをルーティングする"UpcomingDinners"を使用して、URL を生成することです。</span><span class="sxs-lookup"><span data-stu-id="30dc8-168">The difference is that we are generating the URL using the "UpcomingDinners" routing rule we setup within our Global.asax file.</span></span> <span data-ttu-id="30dc8-169">これにより、形式を持つ、Index() アクション メソッドに Url を生成しますお: */Dinners/ページ/{ページ}* {ページ} の値は、変数が提供するという上記現在 PageIndex に基づいて – です。</span><span class="sxs-lookup"><span data-stu-id="30dc8-169">This ensures that we'll generate URLs to our Index() action method that have the format: */Dinners/Page/{page}* – where the {page} value is a variable we are providing above based on the current PageIndex.</span></span>

<span data-ttu-id="30dc8-170">今すぐアプリケーションを実行したときにもう一度おが表示されます、一度に 10 ディナー ブラウザーに。</span><span class="sxs-lookup"><span data-stu-id="30dc8-170">And now when we run our application again we'll see 10 dinners at a time in our browser:</span></span>

![](implement-efficient-data-paging/_static/image5.png)

<span data-ttu-id="30dc8-171">あるも&lt; &lt; &lt;と&gt; &gt; &gt;ナビゲーションを転送をスキップし、上方向へ、データを使用して経由での検索エンジンのアクセス可能な Url を使用すると、ページの下部にある UI:</span><span class="sxs-lookup"><span data-stu-id="30dc8-171">We also have &lt;&lt;&lt; and &gt;&gt;&gt; navigation UI at the bottom of the page that allows us to skip forwards and backwards over our data using search engine accessible URLs:</span></span>

![](implement-efficient-data-paging/_static/image6.png)

| <span data-ttu-id="30dc8-172">**側トピックの内容は、IQueryable の影響を理解する&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="30dc8-172">**Side Topic: Understanding the implications of IQueryable&lt;T&gt;**</span></span> |
| --- |
| <span data-ttu-id="30dc8-173">IQueryable&lt;T&gt;非常に強力な機能をさまざまな興味深い遅延実行のシナリオを有効にする (ページングと構成のように基づくクエリ)。</span><span class="sxs-lookup"><span data-stu-id="30dc8-173">IQueryable&lt;T&gt; is a very powerful feature that enables a variety of interesting deferred execution scenarios (like paging and composition based queries).</span></span> <span data-ttu-id="30dc8-174">として、強力な機能はすべて、使用する方法を慎重にしてはいないに悪用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="30dc8-174">As with all powerful features, you want to be careful with how you use it and make sure it is not abused.</span></span> <span data-ttu-id="30dc8-175">IQueryable を返すことを認識することが重要&lt;T&gt;結果をリポジトリから、連結演算子のメソッドに追加し、そのため、最終的なクエリの実行に参加するための呼び出し元のコードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="30dc8-175">It is important to recognize that returning an IQueryable&lt;T&gt; result from your repository enables calling code to append on chained operator methods to it, and so participate in the ultimate query execution.</span></span> <span data-ttu-id="30dc8-176">この機能は、呼び出し元のコードを提供したくないかどうかは、返す必要があります IList をバックアップ&lt;T&gt;または IEnumerable&lt;T&gt;結果 - は既に実行したクエリの結果が含まれています。</span><span class="sxs-lookup"><span data-stu-id="30dc8-176">If you do not want to provide calling code this ability, then you should return back IList&lt;T&gt; or IEnumerable&lt;T&gt; results - which contain the results of a query that has already executed.</span></span> <span data-ttu-id="30dc8-177">改ページ調整シナリオには、リポジトリ メソッドが呼び出されるとに、実際のデータの改ページ調整ロジックをプッシュするこれが必要です。</span><span class="sxs-lookup"><span data-stu-id="30dc8-177">For pagination scenarios this would require you to push the actual data pagination logic into the repository method being called.</span></span> <span data-ttu-id="30dc8-178">このシナリオでは、FindUpcomingDinners() finder メソッドに返される、PaginatedList がいずれかの署名がある可能性があります更新: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (int pageIndex、int pageSize) {} の戻り値は、IList を戻す&lt;Dinner&gt;、ディナーの合計数を返す"totalCount"out パラメーターを使用して: IList&lt;Dinner&gt; FindUpcomingDinners (int pageIndex、int totalCount out の int pageSize) {}</span><span class="sxs-lookup"><span data-stu-id="30dc8-178">In this scenario we might update our FindUpcomingDinners() finder method to have a signature that either returned a PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize) { } Or return back an IList&lt;Dinner&gt;, and use a "totalCount" out param to return the total count of Dinners: IList&lt;Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize, out int totalCount) { }</span></span> |

### <a name="next-step"></a><span data-ttu-id="30dc8-179">次の手順</span><span class="sxs-lookup"><span data-stu-id="30dc8-179">Next Step</span></span>

<span data-ttu-id="30dc8-180">これで、アプリケーションへの認証と承認をサポートを追加お方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="30dc8-180">Let's now look at how we can add authentication and authorization support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="30dc8-181">[前へ](re-use-ui-using-master-pages-and-partials.md)
[次へ](secure-applications-using-authentication-and-authorization.md)</span><span class="sxs-lookup"><span data-stu-id="30dc8-181">[Previous](re-use-ui-using-master-pages-and-partials.md)
[Next](secure-applications-using-authentication-and-authorization.md)</span></span>