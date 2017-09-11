---
title: "ASP.NET Core でのファイルのアップロード"
author: ardalis
description: "モデル バインディングおよびストリーミングを使用して ASP.NET Core MVC でのファイルをアップロードする方法です。"
keywords: "ASP.NET Core、ファイルのアップロード、モデルのバインディング、ストリーミング IFormFile"
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 78cc9cd846f9b0963dbba9069c86ca295f7a32e4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="3403c-104">ASP.NET Core でのファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="3403c-104">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="3403c-105">によって[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="3403c-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="3403c-106">ASP.NET MVC アクションは、単純なモデル サイズの小さいファイルのバインドまたはよりも大きなファイルのストリーミングを使用して 1 つまたは複数のファイルのアップロードをサポートします。</span><span class="sxs-lookup"><span data-stu-id="3403c-106">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="3403c-107">GitHub から表示またはダウンロードのサンプル</span><span class="sxs-lookup"><span data-stu-id="3403c-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="3403c-108">モデル バインディングで小さなファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="3403c-108">Uploading small files with model binding</span></span>

<span data-ttu-id="3403c-109">小さなファイルをアップロードするには、マルチパート HTML フォームを使用してまたは JavaScript を使用して、POST 要求を作成できます。</span><span class="sxs-lookup"><span data-stu-id="3403c-109">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="3403c-110">例の形式がアップロードされた複数のファイルをサポートするには、Razor を使用して、次に示します。</span><span class="sxs-lookup"><span data-stu-id="3403c-110">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="3403c-111">ファイルのアップロードをサポートするために HTML フォームを指定する必要があります、`enctype`の`multipart/form-data`します。</span><span class="sxs-lookup"><span data-stu-id="3403c-111">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="3403c-112">`files`入力要素の前に示した複数ファイルのアップロードをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="3403c-112">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="3403c-113">省略、`multiple`アップロードする 1 つのファイルだけを許可するこの入力要素の属性です。</span><span class="sxs-lookup"><span data-stu-id="3403c-113">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="3403c-114">としてブラウザーで上記のマークアップを表示します。</span><span class="sxs-lookup"><span data-stu-id="3403c-114">The above markup renders in a browser as:</span></span>

![ファイルのアップロード フォーム](file-uploads/_static/upload-form.png)

<span data-ttu-id="3403c-116">サーバーにアップロードされた個別のファイルからアクセスできます[モデル バインド](xref:mvc/models/model-binding)を使用して、 [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile)インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="3403c-116">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="3403c-117">`IFormFile`次の構造があります。</span><span class="sxs-lookup"><span data-stu-id="3403c-117">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="3403c-118">依存しない、または信頼、`FileName`検証を伴わないプロパティです。</span><span class="sxs-lookup"><span data-stu-id="3403c-118">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="3403c-119">`FileName`プロパティは、表示目的でのみ使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3403c-119">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="3403c-120">モデル バインディングを使用してファイルをアップロードするとき、`IFormFile`インターフェイス、アクション メソッドは、1 つを受け入れることができます`IFormFile`または`IEnumerable<IFormFile>`(または`List<IFormFile>`) を表すいくつかのファイルです。</span><span class="sxs-lookup"><span data-stu-id="3403c-120">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="3403c-121">次の例は、1 つまたは複数のアップロードされたファイルをループ処理、ローカル ファイル システムに保存し、アップロードされたファイルのサイズと合計数を返します。</span><span class="sxs-lookup"><span data-stu-id="3403c-121">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="3403c-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3403c-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span></span>

<span data-ttu-id="3403c-123">使用してアップロードされたファイル、`IFormFile`手法は、バッファー内のメモリまたは web サーバー上のディスクに処理される前にします。</span><span class="sxs-lookup"><span data-stu-id="3403c-123">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="3403c-124">アクション メソッドの内部、`IFormFile`内容はストリームとしてアクセスします。</span><span class="sxs-lookup"><span data-stu-id="3403c-124">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="3403c-125">ローカル ファイル システムだけでなくファイルにストリーミングできる[Azure Blob ストレージ](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs)または[Entity Framework](https://docs.microsoft.com/ef/core/index)です。</span><span class="sxs-lookup"><span data-stu-id="3403c-125">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="3403c-126">Entity Framework を使用してデータベースにバイナリ ファイルのデータを保存して、型のプロパティを定義`byte[]`エンティティで。</span><span class="sxs-lookup"><span data-stu-id="3403c-126">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="3403c-127">型の viewmodel プロパティを指定して`IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="3403c-127">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3403c-128">`IFormFile`使用できますがアクション メソッド パラメーターとして直接または viewmodel プロパティとして、上記のようにします。</span><span class="sxs-lookup"><span data-stu-id="3403c-128">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="3403c-129">コピー、`IFormFile`ストリームをバイト配列に保存します。</span><span class="sxs-lookup"><span data-stu-id="3403c-129">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="3403c-130">ように、パフォーマンスに悪影響は、リレーショナル データベースにバイナリ データを格納する場合の注意を使用します。</span><span class="sxs-lookup"><span data-stu-id="3403c-130">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="3403c-131">ストリーミングの大きいファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="3403c-131">Uploading large files with streaming</span></span>

<span data-ttu-id="3403c-132">サイズまたはファイルのアップロードの頻度には、アプリのリソースの問題が発生する場合は、完全にバッファリングするのではなく、ファイルのアップロードのストリーミングを上に示したモデル バインディングのアプローチは、検討します。</span><span class="sxs-lookup"><span data-stu-id="3403c-132">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="3403c-133">使用しているときに`IFormFile`モデル バインディングがより簡単に解決、およびストリーミングを正しく実装する手順の数が必要です。</span><span class="sxs-lookup"><span data-stu-id="3403c-133">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="3403c-134">64 KB を超える単一バッファー内のファイルは、そのディスク上の一時ファイルをサーバーで RAM から移動されます。</span><span class="sxs-lookup"><span data-stu-id="3403c-134">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="3403c-135">ファイルのアップロードで使用したリソース (ディスク、RAM) は、同時実行ファイルのアップロードのサイズと数によって異なります。</span><span class="sxs-lookup"><span data-stu-id="3403c-135">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3403c-136">ストリーミングはパフォーマンスについて多くはスケールに関する説明。</span><span class="sxs-lookup"><span data-stu-id="3403c-136">Streaming is not so much about perf, it's about scale.</span></span> <span data-ttu-id="3403c-137">バッファーが多すぎますアップロードしようとする場合、メモリまたはディスク領域を使い果たした場合、サイトがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="3403c-137">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="3403c-138">次の例では、コント ローラーのアクションをストリーム配信の JavaScript/角の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3403c-138">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="3403c-139">カスタム フィルター属性を使用して、HTTP ヘッダーの代わりに、要求本文で渡されたファイルの antiforgery トークンが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3403c-139">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3403c-140">アクション メソッドがアップロードされたデータを直接処理されるため、別のフィルターによりモデル バインディングが無効です。</span><span class="sxs-lookup"><span data-stu-id="3403c-140">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="3403c-141">使用して、アクション内で、フォームの内容の読み取りは、 `MultipartReader`、各個人を読み取ります`MultipartSection`ファイルの処理、または適切な内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="3403c-141">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3403c-142">すべてのセクションが読み取られた後、アクションは独自のモデル バインドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3403c-142">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="3403c-143">最初のアクションがフォームを読み込むし、antiforgery トークンを cookie に保存されます (を使用して、`GenerateAntiforgeryTokenCookieForAjax`属性)。</span><span class="sxs-lookup"><span data-stu-id="3403c-143">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="3403c-144">属性が ASP.NET Core の組み込みを使用して[Antiforgery](xref:security/anti-request-forgery)要求トークンを使用して cookie の設定をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3403c-144">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

<span data-ttu-id="3403c-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3403c-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="3403c-146">角が自動的に antiforgery トークンを渡すという名前の要求ヘッダーで`X-XSRF-TOKEN`です。</span><span class="sxs-lookup"><span data-stu-id="3403c-146">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="3403c-147">構成でこのヘッダーを参照する ASP.NET Core MVC アプリが構成されている*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3403c-147">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

<span data-ttu-id="3403c-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3403c-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span></span>

<span data-ttu-id="3403c-149">`DisableFormValueModelBinding`モデルのバインディングを無効にする、次に示す属性が使用される、`Upload`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="3403c-149">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

<span data-ttu-id="3403c-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3403c-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="3403c-151">モデル バインディングが無効になるので、`Upload`アクション メソッド パラメーターを受け取らないです。</span><span class="sxs-lookup"><span data-stu-id="3403c-151">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="3403c-152">直接連携して、`Request`プロパティ`ControllerBase`です。</span><span class="sxs-lookup"><span data-stu-id="3403c-152">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="3403c-153">A`MultipartReader`各セクションの読み取りに使用します。</span><span class="sxs-lookup"><span data-stu-id="3403c-153">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3403c-154">GUID ファイル名を持つファイルが保存され、内のキー/値のデータが格納されている、`KeyValueAccumulator`です。</span><span class="sxs-lookup"><span data-stu-id="3403c-154">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3403c-155">すべてのセクションが読み込まれた後の内容、`KeyValueAccumulator`フォーム データをモデルの種類にバインドするために使用します。</span><span class="sxs-lookup"><span data-stu-id="3403c-155">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3403c-156">完全な`Upload`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3403c-156">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="3403c-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3403c-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3403c-158">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="3403c-158">Troubleshooting</span></span>

<span data-ttu-id="3403c-159">下には、ファイルとその考えられる解決策のアップロードを扱うときの一般的な問題を示します。</span><span class="sxs-lookup"><span data-stu-id="3403c-159">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="3403c-160">IIS での予期しないが見つかりません。 エラー</span><span class="sxs-lookup"><span data-stu-id="3403c-160">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="3403c-161">次のエラーを示します、ファイルのアップロードを超える、サーバーの構成済み`maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="3403c-161">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3403c-162">既定の設定は`30000000`、約 28.6 MB であります。</span><span class="sxs-lookup"><span data-stu-id="3403c-162">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="3403c-163">値を編集してカスタマイズできる*web.config*:</span><span class="sxs-lookup"><span data-stu-id="3403c-163">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="3403c-164">この設定は、IIS にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="3403c-164">This setting only applies to IIS.</span></span> <span data-ttu-id="3403c-165">動作は、Kestrel でホストしているときに、既定では発生しません。</span><span class="sxs-lookup"><span data-stu-id="3403c-165">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3403c-166">詳細については、次を参照してください。[要求制限\<requestLimits\>](https://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits)です。</span><span class="sxs-lookup"><span data-stu-id="3403c-166">For more information, see [Request Limits \<requestLimits\>](https://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3403c-167">IFormFile を使用して null 参照例外</span><span class="sxs-lookup"><span data-stu-id="3403c-167">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3403c-168">コント ローラーが受け入れる場合を使用してファイルをアップロード`IFormFile`値が常に null を検索、HTML フォームを指定することを確認するが、`enctype`の値`multipart/form-data`です。</span><span class="sxs-lookup"><span data-stu-id="3403c-168">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3403c-169">この属性に設定されていない場合、`<form>`要素、ファイルのアップロードは行われませんと任意のバインド`IFormFile`引数は null になります。</span><span class="sxs-lookup"><span data-stu-id="3403c-169">If this attribute is not set on the `<form>` element, the file upload will not occur and any bound `IFormFile` arguments will be null.</span></span>