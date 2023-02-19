{
// stack 자료 구조
// Last In First Out : 나중에 들어온 것이 가장 처음으로 나올 수 있게 해주는 자료구조

interface Stack<T> {
readonly size: number;
push(value: T): void;
pop(): T;
}

type StackNode<T> = {
readonly value: T;
readonly next?: StackNode<T>;
};

class StackImpl<T> implements Stack<T> {
private \_size: number = 0;
private head?: StackNode<T>;
get size() {
return this.\_size;
}

    constructor(private capacity: number) {}

    push(value: T) {
      if (this.size === this.capacity) {
        throw new Error("stack is full");
      }
      // const node: StackNode<T> = { value, next: this.head };
      const node = { value, next: this.head }; // 이미 head에 정확한 타입 StackNode<T>라고 명시되어있기 때문에 여기에서는 타입 추론을 이용하여 이와 같이 줄여서 사용도 가능하다 이처럼 타입이 명확한 경우 타입추론을 이용하여 코드를 정리할 수 있다.
      this.head = node;
      this._size++;
    }

    pop(): T {
      if (this.head == null) {
        throw new Error("Stack is empty");
      }
      const node = this.head;
      this.head = node.next;
      this._size--;
      return node.value;
    }

}

const stack = new StackImpl<string>(10);
stack.push("yxxn");
stack.push("bob");
stack.push("steve");

while (stack.size !== 0) {
console.log(stack.pop());
}

const stack2 = new StackImpl<number>(10);
stack2.push(1);
stack2.push(2);
stack2.push(3);

while (stack2.size !== 0) {
console.log(stack2.pop());
}
}
