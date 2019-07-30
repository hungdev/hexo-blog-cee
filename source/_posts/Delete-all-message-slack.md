---
title: Delete all message slack
date: 2019-07-28 18:31:20
tags:
---
Có khi nào bạn muốn tất cả các message trong slack của mình bay mất khỏi channel?
Mình nghĩ cứ tưởng chỉ có mỗi mình mình cần nó thôi, nhưng khi quay ra search google thì cũng có nhiều người cần như mình =)))
Bằng chứng nè: 
<center>{% asset_img align-center delete-mess-slack-normal-version.png "delete-mess-slack-admin-edition-version" %}</center>

<span style="color:red">Rồi tiếp nè</span>

<center>{% asset_img align-center delete-mess-slack-admin-edition-version.png "delete-mess-slack-admin-edition-version" %}</center>


Đó, cái bản admin edition 1 củ khoai kia cũng 235 người download. Nhiều chưa!!!!
Đấy, có phải mỗi mình tui cần đâu =)))

Nhưng search được cái extension, cài thử xong thì cũng không xóa được, cay cú nghĩ thôi, tự viết cái.

Tiếp lên google search có bài viết về delete message slack, cũng có hẳn 1 lib viết = python. Nhưng mình lại ko phải là dev python nên mình cũng không máu chơi nó lắm, mặc dù mình thử nó thì nó cũng chạy ổn phết.

Vậy nên mình chơi node.

Lúc đầu mình định chơi slack webhook nhưng hình như nó không hỗ trợ remove message thì phải nên mình chuyển qua search api remove message.

search qua lại thì mình cũng thấy có 1 bài viết khá chi tiết:
https://medium.com/@jjerryhan/cleaning-all-messages-on-slack-channel-c46d71615c9a

và đây mình dùng đoạn snipet này:

```
https://gist.githubusercontent.com/firatkucuk/ee898bc919021da621689f5e47e7abac/raw/8c3b420fe3e334d740957a229937cdcbd10c0063/delete-channel-messages.js
```

Để chạy được nó bạn cần token và channel id.

Đợt trước mình cũng có tìm hiểu slack auth và đã làm cái lib [react-native-slack-login](https://www.npmjs.com/package/react-native-slack-login), mình lấy luôn token của nó, nhưng nó hơi mất thời gian, nên mình đã dùng [legency token](https://api.slack.com/custom-integrations/legacy-tokens).
Với legency token nếu bạn là admin của cái workspace slack đó thì bạn có thể xóa được message của người khác nữa, còn không thì bạn chỉ xóa được message của chính bạn trong channel đó thôi.

Xong bước tìm token.
Đến bước tìm channel id. 
Bạn vô channel slack bằng browser, bạn sẽ thấy link nó dạng như này
`https://app.slack.com/client/TLU6UGZ7S/CLW2R0DU7`

thì channel id sẽ là : `CLW2R0DU7`  ( chuỗi ký tự sau / cuối cùng )

Nếu bạn có gặp lỗi này
```
/Users/name/Scripts/delete-channel-messages.js:66
        for (var i = 0; i < response.messages.length; i++) {
                                              ^

TypeError: Cannot read property 'length' of undefined
    at IncomingMessage.<anonymous> (/Users/name/Scripts/delete-channel-messages.js:66:47)
    at IncomingMessage.emit (events.js:187:15)
    at endReadableNT (_stream_readable.js:1091:14)
    at process._tickCallback (internal/process/next_tick.js:174:19)
```

thì bạn giống mình rồi đó, =)))
Lần đầu mình cũng bị lỗi này, nó không có message, trường hợp của mình do sai token và channel id nên nó không fetch được data.
Bạn cần check lại 2 cái đó nhé, nếu gặp case này.

Done! Đó là tất cả những gì mình gặp khi remove message slack.
