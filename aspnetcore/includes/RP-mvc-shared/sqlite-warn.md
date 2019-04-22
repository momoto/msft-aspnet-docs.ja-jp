---
ms.openlocfilehash: 1f8d3913c83aaf5fe6ec2cec482a30f0f066c16b
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841694"
---

> [!NOTE]
> このチュートリアルでは、可能な場合は Entity Framework Core の "*移行*" 機能を使います。 移行では、データ モデルの変更に合わせてデータベース スキーマが更新されます。 ただし、移行では EF Core プロバイダーでサポートされている種類の変更しか行うことができず、SQLite プロバイダーの機能は制限されています。 たとえば、列の追加はサポートされていますが、列の削除や変更はサポートされていません。 列を削除または変更するために移行を作成した場合、`ef migrations add` コマンドは成功しますが、`ef database update` コマンドは失敗します。 これらの制限により、このチュートリアルでは SQLite のスキーマの変更に対しては移行を使用しません。 代わりに、スキーマを変更するときは、データベースを削除して再作成します。
>
>SQLite の制限に対する回避策としては、テーブル内の何かが変更されたときにテーブルの再構築を実行する移行コードを、手動で作成します。 テーブルの再構築には次の作業が含まれます。
>
>* 既存のテーブルの名前の変更。
>* 新しいテーブルの作成。
>* 古いテーブルから新しいテーブルへのデータのコピー。
>* 古いテーブルの削除。
>
>詳細については、次のリソースを参照してください。
>
> * [SQLite EF Core データベース プロバイダーの制限事項](/ef/core/providers/sqlite/limitations)
> * [移行コードをカスタマイズする](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [データのシード処理](/ef/core/modeling/data-seeding)