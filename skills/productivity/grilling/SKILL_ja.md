---
name: grilling
description: plan、decision、または idea について、ユーザーを徹底的に問い詰めます。ユーザーが自分の考えを stress-test したい場合、または 'grill' trigger phrases を使った場合に使用します。
---

共有理解に達するまで、ユーザーに徹底的に interview してください。これを **design tree** として map します：すべての decision は、その下にぶら下がる decisions へと分岐します。

tree は **rounds** で進めます。**frontier** とは、prerequisites がすでに確定しているすべての decision です。つまり、まだ聞いていない答えを推測せずに、_今_ 尋ねられる questions です。1 つの round で frontier 全体を尋ねてください：各 question に番号を付け、あなたの recommended answer を示します。その後、次の round に進む前にユーザーの answers を待ちます。

round は次の形式で示してください：

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

各 question は、「はい」と答えればあなたの recommended answer を受け入れたことになるように表現してください。

各 round でユーザーが答えるたびに tree は形を変えます：確定した decisions は frontier を外側へ押し出し、それに依存していた questions を unblock します。frontier を再計算し、次の round を尋ねてください。この round でまだ open な別の question に answer が依存する question は、この round ではなく _later_ round に属します。

_facts_ を見つけるのはユーザーではなく、あなたの仕事です。frontier question が environment（filesystem、tools など）からの fact を必要とする場合は、新しい context window で sub-agent を dispatch して見つけさせてください。自分で調べられることをユーザーに尋ねてはいけません。それを待って block しないでください：running exploration は unsettled prerequisite なので、それに downstream の questions だけが sub-agent の report を待ちます。frontier の残りは今すぐ尋ねてください。_decisions_ はユーザーのものです：それぞれをユーザーに提示し、待ってください。

frontier が空になったとき、session は完了です：design tree のすべての branch が訪問され、暗黙に仮定されたものが何も残っていない状態です。共有理解に達したことをユーザーが確認するまでは、それに基づいて行動してはいけません。
