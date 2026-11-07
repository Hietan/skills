# Hietan Workflows

GitHubを原本として管理する、共有ワークフローPluginです。Plugin名は `hietan-workflows`、marketplace名は `hietan-skills` です。

## 含まれるSkill

- [git-commit-workflow](skills/git-commit-workflow/SKILL.md): コミットの書式、AI共同著者、コミットするタイミング。
- [repository-management](skills/repository-management/SKILL.md): ghqによる取得と `~/Documents/ghq/GitHub/`・`Overleaf/` への配置。

各Skillの原本は `skills/<skill-name>/SKILL.md` の一か所です。従来のリポジトリ直下から移動しました。

## Codex CLIで利用する

以下のコマンドに対応するCodex CLIで実行します。

```sh
codex plugin marketplace add Hietan/skills --ref main
codex plugin add hietan-workflows@hietan-skills
```

インストール後は新しいセッションで利用します。CLIのGit取得はPlugin配布機構のキャッシュであり、作業用リポジトリのクローンとは別です。

更新時は次を実行し、Plugin一覧とインストール状態を確認してください。必要ならPluginを再インストールし、新しいセッションを開始します。

```sh
codex plugin marketplace upgrade hietan-skills
codex plugin list --marketplace hietan-skills
```

## ChatGPT・Workのデスクトップで利用する

このリポジトリを作業プロジェクトとして開くと、対応クライアントでは `.agents/plugins/marketplace.json` がrepo marketplaceとして利用できます。アプリを再起動し、Pluginsから **Hietan Skills** を選び、**Hietan Workflows** をインストールしてください。ローカルの発見・インストールだけでは、他端末やWebへの配布は完了しません。

Web・モバイルを含むワークスペース配布では、管理者がGitHub marketplaceとして `https://github.com/Hietan/skills` を取り込みます。Pathは空、追跡するブランチは `main` とします。同期状況・利用権限・インストール状態を確認してください。日次同期と手動同期が利用できます。

個人用配布やdotsの対応状況は利用環境で確認してください。このGitHubリポジトリへの公開は、共通の公開Pluginディレクトリへの審査・登録とは別です。全アカウント・全端末への自動反映は保証しません。

## 適用と保守

Skillは依頼に応じて選択されます。必要時はSkillを明示的に指定してください。毎回の適用を促す場合は、各環境の共通指示へ次を追加します。

> Gitリポジトリの取得・配置にはrepository-managementを、ファイル変更後のコミットにはgit-commit-workflowを適用する。

Skillを追加するときは `skills/<skill-name>/SKILL.md` を追加します。変更時は `plugin.json` のversionも更新し、検証後にpushします。既存セッションへの即時反映は前提にしません。

標準のAgent Plugins 1.0形式を使用しています。各ホストで対応状況を確認してください。MCPサーバー、認証接続、hooksは含みません。

## 公式資料

- [Pluginのパッケージ化とmarketplace](https://developers.openai.com/plugins/build/plugins)
- [GitHubからのワークスペース同期](https://learn.chatgpt.com/docs/enterprise/plugin-management)
