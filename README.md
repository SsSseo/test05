### [과제] 숙련주차 과제 답


Q1. 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음.

<문제>
- onSubmitHandler 안에서는 추가 버튼을 눌러도 todos에 리스트가 추가 되는 로직이 없다.

<해결>
- 리스트가 추가되는 로직은 moduls / todos.js에 있다.
- moduls / todos.js 로 값을 보내기 위해 dispatch 추가한다.

<수정 코드>
features / todos / conponents / Form.jsx

const dispatch = useDispatch();  // 추가
const onSubmitHandler = (event) => {
    event.preventDefault();

    if (todo.title.trim() === "" || todo.body.trim() === "") return;
    
    dispatch(addTodo({ ...todo, id }));  // 추가
    setTodo({
      id: 0,
      title: "",
      body: "",
      isDone: false,
    });
  };




Q2. 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.

<문제>
- todos에 새로 들어온 todo를 추가하는것이 아닌 기존의 todos를 새로운 todo로 교체하는 코드이다
- todos: [action.payload] 은 todos 의 요소를 새로 받아 온 payload로 바꾸는 것이다.

<해결>
- state에 저장되어 있던 todos를 복사 후 새로 들어온 payload(todo)를 추가한다.

<수정 코드>
redux / moduls / todos.js

case ADD_TODO:
  return {
    ...state,
    todos: [...state.todos, action.payload], // 수정
  };



Q3. 삭제 기능이 동작하지 않음.

<문제>
- reducer 에 삭제 기능 구현되어있지 않음

<해결>
- features / todos / conponents / List.jsx에서 id를 dispatch로 보내주고있다
- deleteTodo creator에서 payload에 받아 온 id를 저장했다.
- reducer에서 삭제버튼이 눌린 todo의 id와 같은 id를 가진 todo를 dotos에서 filter를 사용하여 삭제한다.

<수정 코드>
redux / moduls / todos.js

case DELETE_TODO:
  return {
    ...state,
    todos: state.todos.filter(todo => todo.id !== action.payload)
  } // 추가



Q4. 상세 페이지에 진입하였을 때 데이터가 업데이트 되지 않음.
<문제>
- reducer 에 상세페이지에서 보여 줄 todo를 저장하는 코드가 있는데 Detail.jsx 파일은 reducer와 연결이 되어있지 않다.

<해결>
- dispatch를 사용하여 클릭 한 todo의 id를 modules / todos.js 보낸다.
- creator에서 payload에 id가 저장이 되고 reducer에서 
    case GET_TODO_BY_ID:
      return {
        ...state,
        todo: state.todos.find((todo) => {
          return todo.id === action.payload;
        }),
      };
  이 부분을 통해 동일한 id를 가진 todo 내용을 todo 상태에 저장한다.

<수정 코드>
pages / Detail.jsx

useEffect(() => {
  dispatch(getTodoByID(id));
}, [dispatch, id]);  // 추가



Q5. 완료된 카드의 상세 페이지에 진입하였을 때 올바른 데이터를 불러오지 못함.
<문제>
- 상세보기를 클릭하면 링크 이동을 할 때 todo.id를 포함시켜 넘겨줘야하는데 index로 잘못 넘겨주고 있다.

<수정 코드>
features / todos / conponents / List.jsx
<StLink to={`/${todo.id}`} key={todo.id}> // index -> todo.id 수정




Q6. 취소 버튼 클릭시 기능이 작동하지 않음.
<문제>
- 완료 상태의 살세보기 버튼은 onToggleStatusTodo 함수를 호출하기만 하고 해당 id값을 넘겨주지 않았다.

<해결>
- 해당 todo의 id를 함께 넘겨준다.

<수정 코드>
features / todos / conponents / List.jsx
<StButton
  borderColor="green"
  onClick={() => onToggleStatusTodo(todo.id)} // 수정
>


Q6. 취소 버튼 클릭시 기능이 작동하지 않음.
<문제>