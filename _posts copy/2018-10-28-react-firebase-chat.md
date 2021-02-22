---
layout: post
comments: true
title: '[REACT] React와 Firebase로 간단한 채팅앱 만들기 '
categories: [react]
tags: [react]
date: 2018-10-28
---

![headerimg](/assets/img/subcate/react.gif)
`react`의 기술적인 분석을 이야기합니다.
{:.center}

## React-Firebase-Chat

저번 주말에 저는 **firebase-codelabs**에서 제공해주는 강의를 보고 채팅앱을 react 로 만들어 보았습니다!<br>
firebase 의 firestore 가 나온 후에 개발공부를 본격적으로 시작한 저는 그 이전에 많이 쓰이던 realtime database 를
써볼 기회가 없던 저에게 아주 좋은 공부가 되었죠.

[FriendlyChat 코드랩 강의](https://codelabs.developers.google.com/codelabs/firebase-web/#0)

[React-Firebase-Chat 전체 코드](https://github.com/huurray/react-firebase-chat)

![image](/assets/img/post/react-firebase-chat/0.png)

채팅앱 **호스팅 페이지**도 올려놓을게요! 사용해보세요!

[React-Firebase-Chat 앱 페이지](https://react-firebase-chat-3acad.firebaseapp.com/)

## Firebase Realtime Database 를 선택한 이유

**1. 서버비용**<br>
realtime database 의 사용료 책정은 데이터 용량 기반이고 firestore 의 사용료는 요청 횟수이기 때문에 <br>
많은 요청이 실시간으로 이루어 지는 채팅앱의 경우 realtime db 가 더 효율적이라고 판단하였습니다.

**2. firebase codelab 의 추천**<br>
codelab 또한 firestore 대신 realtime db 를 사용하는 것을 추천한다.<br>
(이유가 상세히 나와있으면 좋을텐데 없음.. 뭐가 좋은지 알려주실분..)

## 주요 메서드

**1. 메세지 보내기**<br>

```javascript
  async onSendMessege() {
    if (!firebase.auth().currentUser) {
      return Alert.error('로그인 후 이용가능합니다.');
    }

    if (!this.state.inputValue) {
      return Alert.error('메시지를 입력해주세요.');
    }
    try {
      await firebase
        .database()
        .ref('/messages/')
        .push({
          creatorId: firebase.auth().currentUser.uid,
          displayName: this.props.user.displayName,
          message: this.state.inputValue,
          photoURL: this.props.user.photoURL,
          createdAt: new Date().getTime()
        });
      this.setState({ inputValue: '' });
    } catch (err) {
      console.error('Error writing new message', err);
    }
  }
```

database 는 **push** 또는 **set**함수를 통해 데이터를 업로드합니다. **update**는 기존 데이터베이스에 저장되있는 값만 교체하죠!<br>
저는 로그인을 통해서 얻은 **이름**과 **프로필사진**과 **uid**, input 을 통해 얻은 **메세지값**, 그리고 **현재시간**을 저장해 주었습니다.

**2. 이미지 보내기**<br>

```javascript
  async onFileSelected(e) {
    e.preventDefault();
    const file = e.target.files[0];

    if (!firebase.auth().currentUser) {
      return Alert.error('로그인 후 이용가능합니다.');
    }
    if (!file.type.match('image.*')) {
      return Alert.error('사진파일만 보낼 수 있어요.');
    }
    if(file.size > 2000000) {
      return Alert.error('사이즈는 3mb까지 가능합니다.');
    }

    try {
      const messageRef = await firebase.database().ref('/messages/');

      const filePath =
        firebase.auth().currentUser.uid + '/messages/' + file.name;
      const fileSnapshot = await firebase
        .storage()
        .ref(filePath)
        .put(file);
      const url = await fileSnapshot.ref.getDownloadURL();

      messageRef.push({
        displayName: this.props.user.displayName,
        imageMsgURL: url,
        photoURL: this.props.user.photoURL,
        creatorId: this.props.user.uid,
        createdAt: new Date().getTime()
      });
    } catch (err) {
      console.log(err);
    }
  }
```

**file input** 을 사용하였습니다. 여러 조건식을 이용하여 이미지파일 only, 사이즈 제한 등을 처리해주었습니다.<br>
firebase storage 에 원하는 경로로 파일을 업로드 해줍니다. 이때는 **put**함수를 사용해 주었습니다. put 함수에는 **파일이나 blob 형식**이 위치할 수 있습니다!<br>
**async/await** 를 사용하여 대기한 후 이미지가 업로드되고 URL 을 가져오면 데이터메이스에 이미지 메세지를 저장해 줍니다!

**3. 메세지 가져오기**<br>

```javascript
  loadChatCard() {
    const messagesRef = firebase
      .database()
      .ref('/messages/')
      .limitToLast(100);
    const callback = snap => {
      const data = snap.val();
      this.setState(prevState => ({
        messages: [
          ...prevState.messages,
          {
            ...data,
            key: snap.key
          }
        ]
      }));
    };

    messagesRef.on('child_added', callback);
    messagesRef.on('child_changed', callback);
  }
```

**on** 함수를 사용해줘서 데이터베이스의 값이 **'child_added'**, **'child_changed'**되었을 때마다 값을 읽어오도록 설정합니다.

```javascript
  showChatCards() {
    return this.props.messages.map((m, i) => {
      let me = false;
      if (this.props.user.uid === m.creatorId) me = true;

      const isPhotoURL = m.photoURL || require('./img/profile_placeholder.png');

      const relativeTime = moment(m.createdAt).format('LLL');

      return (
        <Card key={m.key} me={me}>
          <ProfileImg src={isPhotoURL} />
          {m.message && (
            <TextBox>
              <Message me={me}>{m.message}</Message>
              <Name me={me}>
                {m.displayName}, {relativeTime}
              </Name>
            </TextBox>
          )}
          {m.imageMsgURL && (
            <ImgBox>
              <ImgMessage src={m.imageMsgURL} />
            </ImgBox>
          )}
        </Card>
      );
    });
```

가져온 메세지 배열을 카드형식으로 뿌려주면 되겠죠?

```javascript
  componentDidMount() {
    const messageBody = this.chatCards.current;
    messageBody.scrollTop = messageBody.scrollHeight;
  }

  componentDidUpdate() {
    const messageBody = this.chatCards.current;
    messageBody.scrollTop = messageBody.scrollHeight;
  }

  render() {
    return <Container ref={this.chatCards}>{this.showChatCards()}</Container>;
  }
```

메세지가 길어진다면 css 에 **overflowY: auto** 를 설정해서 스크롤로 표현해야 합니다.
또 **scrollTop** 을 항상 bottom 에 넣기 위해 위와 같이 작성합니다.
