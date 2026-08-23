# 【Claude Codeへの指示】講座運営AIキット（3スキル）の導入

このファイルを読んだAIへ。以下の手順で進めてください。

> ⚠️ **重要①：確認を取る前に、勝手にインストールしないこと。**
> ステップ2で担当者から明確な「はい」をもらうまで、ファイルのコピーや設定変更は一切しないでください。
>
> ⚠️ **重要②：既存のデータ・既存のスキルを絶対に消さないこと。**
> この導入で行う変更は「`~/.claude/skills/` の下に新しいフォルダを3つ作って、それぞれにSKILL.mdを1つ置く」だけです。
> - `rm` / `rm -rf` / `mv` は**一切使わない**（`mkdir -p` と `cp` だけで完結します）
> - `~/.claude/skills/` の中にある**他のスキルフォルダには一切触らない**（中身を見る必要もありません）
> - `~/.claude/settings.json`・`CLAUDE.md`・メモリ等、既存の設定ファイルは**書き換えない**
> - 同名のスキルが**すでに存在した場合**は、上書きせずに担当者に
>   「同名のスキルがすでにあります。上書きしますか？（既存の内容は消えます）」と確認してから進めること
>
> ⚠️ **重要③：3つ全部を「今すぐ使え」と勧めないこと。**
> 導入は3つまとめてで構いませんが、**使い始めるのは1つずつ**です。ステップ5で必ず順番を案内してください。

> 💻 **Mac / Windows 共通で使えます。** コマンドはOSごとに書き分けてあるので、担当者の環境に合う方を使ってください。
> - **Mac / Linux**、または Windows で **Git Bash** が使える場合 → 「Mac / Linux」の欄
> - **Windows で PowerShell** の場合 → 「Windows」の欄
> - 迷ったら、まず「Mac / Linux」の欄を実行してみて、`command not found` 等で失敗したら Windows 欄に切り替える

---

## ステップ0：Zipのまま渡された場合（解凍済みなら飛ばす）

担当者が **`my-koza-ai-kit.zip` をそのまま渡してきた**場合は、先に解凍します。

1. 担当者に「Zipを解凍してよいですか？」と確認する
2. **Zipと同じ場所に**解凍する（既存フォルダを上書きしないよう、解凍先は必ず新しいフォルダにする）

   **Mac / Linux**
   ```
   unzip "<zipのパス>" -d "<zipがあるフォルダ>"
   ```
   **Windows（PowerShell）**
   ```
   Expand-Archive -Path "<zipのパス>" -DestinationPath "<zipがあるフォルダ>"
   ```
3. 出てきた `my-koza-ai-kit` フォルダの中の、このファイルの場所を控えて、ステップ1へ進む

---

## ステップ1：まず内容を説明する（インストールはまだしない）

担当者に、これから入れるものを次の内容で説明してください。

> 「講座運営を助けるスキルを3つ導入できます。
>
> **① フィードバック下書き（`/my-client-feedback`）**
> 毎月届く受講生の素材（写真・動画・報告フォーム）を読んで、前回との差分を出し、フィードバック文の下書きまで作ります。
>
> **② 電子書籍リメイク（`/my-ebook-remake`）**
> 昔作った電子書籍や無料PDFを、今のお客さん像に合わせて作り直します。ゼロからではなく、旧版を読み込んで土台にします。
>
> **③ 面談準備（`/my-meeting-prep`）**
> 面談の前に相手の記録を集めて当日メモを作ります。面談後は文字起こしを渡すと、約束・宿題・申し送りを記録します。
>
> **入る場所**：`~/.claude/skills/` の下にフォルダが3つ増えるだけです
>
> **必要なもの**：Claude Codeだけです（Chrome拡張などは不要）
>
> **このキットがやること／やらないこと**：
> - やる：素材を整理する／下書きを作る／記録を残す
> - やらない：**あなたの確認なしに誰かへ送信すること**／既存のファイルやスキルの削除・上書き／効果を断定する文章を書くこと
>
> ⚠️ **3つ入りますが、使い始めるのは1つずつです。** おすすめの順番は導入後にお伝えします。
>
> なお、どのスキルも**初回に「普段どうやっているか」を一問一答で聞きます。**そこで聞いた内容をもとに、あなた専用の設定として保存します。」

## ステップ2：導入してよいか確認を取る

上の説明のあと、こう聞いてください：

> 「この3つのスキルを `~/.claude/skills/` に導入してよろしいですか？（はい／いいえ）
> 3つのうち一部だけ入れることもできます。その場合はどれを入れるか教えてください。」

- **「はい」以外**（いいえ・保留・質問だけ）の場合は、**導入せずに待つ**。質問には答えてよいが、勝手に進めない
- **「①だけ入れたい」など一部指定**の場合は、指定されたものだけをコピーする
- 「はい」をもらったらステップ3へ

## ステップ3：導入する

「はい」をもらってから、以下を実行してください。やることは「**一覧を見る → フォルダを作る → ファイルをコピーする**」だけ。削除・移動系のコマンドは使いません。

※ `<このキットのパス>` は、担当者がZipを解凍した場所（`my-koza-ai-kit` フォルダ）に読み替えてください。

### 3-0. 同名スキルがすでにないか確認する

**Mac / Linux**
```
ls ~/.claude/skills/ 2>/dev/null
```
**Windows（PowerShell）**
```
Get-ChildItem "$env:USERPROFILE\.claude\skills" -ErrorAction SilentlyContinue
```

- `my-client-feedback` / `my-ebook-remake` / `my-meeting-prep` が既にある → 「同名のスキルがすでにあります。上書きしますか？」と確認してから次へ
- 他のスキルが並んでいても**それらには一切触らない**

### 3-1. コピー先フォルダを作る（既存フォルダは消えません）

**Mac / Linux**
```
mkdir -p ~/.claude/skills/my-client-feedback ~/.claude/skills/my-ebook-remake ~/.claude/skills/my-meeting-prep
```
**Windows（PowerShell）**
```
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\my-client-feedback","$env:USERPROFILE\.claude\skills\my-ebook-remake","$env:USERPROFILE\.claude\skills\my-meeting-prep"
```

### 3-2. SKILL.md をコピーする

**Mac / Linux**
```
cp "<このキットのパス>/my-client-feedback/SKILL.md" ~/.claude/skills/my-client-feedback/SKILL.md
cp "<このキットのパス>/my-ebook-remake/SKILL.md"    ~/.claude/skills/my-ebook-remake/SKILL.md
cp "<このキットのパス>/my-meeting-prep/SKILL.md"    ~/.claude/skills/my-meeting-prep/SKILL.md
```
**Windows（PowerShell）**
```
Copy-Item "<このキットのパス>\my-client-feedback\SKILL.md" "$env:USERPROFILE\.claude\skills\my-client-feedback\SKILL.md"
Copy-Item "<このキットのパス>\my-ebook-remake\SKILL.md"    "$env:USERPROFILE\.claude\skills\my-ebook-remake\SKILL.md"
Copy-Item "<このキットのパス>\my-meeting-prep\SKILL.md"    "$env:USERPROFILE\.claude\skills\my-meeting-prep\SKILL.md"
```

### 3-3. コピーできたか確認する

**Mac / Linux**
```
ls -la ~/.claude/skills/my-client-feedback/ ~/.claude/skills/my-ebook-remake/ ~/.claude/skills/my-meeting-prep/
```
**Windows（PowerShell）**
```
Get-ChildItem "$env:USERPROFILE\.claude\skills\my-client-feedback","$env:USERPROFILE\.claude\skills\my-ebook-remake","$env:USERPROFILE\.claude\skills\my-meeting-prep"
```

### 3-4. 完了を伝える
> 「導入できました。**Claude Codeを一度再起動**してください。」

---

## ステップ4：使う順番を案内する ← ここを飛ばさない

再起動を伝えたあと、**必ず**次を伝えてください。

> 「3つ入りましたが、**同時に始めないでください。** 一度に全部やろうとすると、だいたい全部中途半端になります。
>
> **おすすめの順番はこれです。**
>
> **1️⃣ まず `/my-client-feedback`（フィードバック下書き）から**
> 毎月必ず発生する作業なので、効果がすぐ分かります。ここで『AIに任せるとラクになる』感覚を掴むと、残り2つが一気に早くなります。
>
> **2️⃣ 次に `/my-ebook-remake`（電子書籍リメイク）**
> 売上の入り口なので効きますが、①で操作に慣れてから取りかかった方が途中で止まりません。
>
> **3️⃣ 最後に `/my-meeting-prep`（面談準備）**
> ①②と違って毎日使うものではないので、後回しで構いません。**面談の回数が少なくても入れておく価値はあります。**回数が少ないうちに型を作っておくと、増えたときにそのまま回りますし、記録が溜まること自体が資産になります。
>
> まずは①を1回動かしてみてください。」

## ステップ5：初回セットアップがあることを伝える

> 「どのスキルも、**初めて動かすときにあなたのやり方を質問してきます。**
> 面倒に感じるかもしれませんが、ここが一番大事なところです。丁寧に答えるほど、2回目以降の精度が変わります。
>
> 特に①では『**素材のどこを、どの順番で見ていますか？**』『**「順調」と思う基準は何ですか？**』のように、
> 普段の仕事の中身を1つずつ聞かれます。そのあとで『今までに書いたフィードバックを2〜3個貼ってください』と続きます。
>
> **一問一答で進むので、うまくまとめて話せなくても大丈夫です。**答えにくいときは具体例を出して聞き直してくれます。」

---

## うまくいかないとき

| 症状 | 対処 |
|---|---|
| `/my-client-feedback` 等が出てこない | Claude Codeを再起動したか確認。スキルフォルダにSKILL.mdがあるか確認（3-3のコマンド） |
| `mkdir`/`cp` が動かない | Windowsでbashが使えない環境。PowerShell用のコマンド（各手順の「Windows」欄）に切り替える |
| `~` のパスが見つからないと言われる | Windowsでは `~` が使えないことがある。`$env:USERPROFILE` に置き換える |
| 一部だけ入れたい | 3-1・3-2で該当するスキルの行だけ実行すればよい |
| 3つとも使いこなせる気がしないと言われる | 「①だけで大丈夫です」と伝える。無理に3つ使わせない |

## アンインストール

担当者から**明確に「消して」と言われたときだけ**、以下を実行する。

**Mac / Linux**
```
rm -rf ~/.claude/skills/my-client-feedback
rm -rf ~/.claude/skills/my-ebook-remake
rm -rf ~/.claude/skills/my-meeting-prep
```
**Windows（PowerShell）**
```
Remove-Item -Recurse "$env:USERPROFILE\.claude\skills\my-client-feedback"
Remove-Item -Recurse "$env:USERPROFILE\.claude\skills\my-ebook-remake"
Remove-Item -Recurse "$env:USERPROFILE\.claude\skills\my-meeting-prep"
```

- 消してよいのは**この3つのフォルダだけ**。`.claude/skills/` そのものや他のスキルフォルダは絶対に消さない
- 実行前に必ず「この3つのフォルダだけを削除します。よろしいですか？」と確認する
- **一部だけ消したい場合は、該当する行だけ実行する**
