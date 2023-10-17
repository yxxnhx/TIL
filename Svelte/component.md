# 컴포넌트 간 통신하는 다양한 방법

스벨트 컴포넌트 간 통신하는 방법은 여섯 가지가 있다.

### props

부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때 사용한다.
bind를 사용하면 부모 컴포넌트에도 데이터를 전달할 수 있다.

### slot

부모 컴포넌트가 자식 컴포넌트로 콘텐츠를 전달해서 자식 컴포넌트가 이 콘텐츠로 무엇을, 어디에, 어떻게 화면에 그릴지 결정하게 만들 수 있다.

### event

자식 컴포넌트에서 발생한 일을 부모 컴포넌트에 알릴 때 쓸 수 있다.
이벤트에 전달할 때 이벤트 객체에 필요한 데이터를 함께 보낼 수도 있다.

### context

조상 컴포넌트가 후손 컴포넌트에 데이터를 전달하고 싶을 때, 그 사이의 모든 계층에 데이터를 명시적로 전달할 필요 없이 바로 전달하는 방법이다.

### module context

데이터를 컴포넌트 모듈에 저장해서 컴포넌트의 모든 인스턴스가 쓸 수 있게 한다.

### store

컴포넌트 바깥에 데이터를 저장하고 누구나 쓸 수 있게 한다.