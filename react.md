Всплытие и погружение

- Event Loop
- Замыкание, лексическое окружение
- var / let / const отличия
- DOM VDOM

```ampcss
При быстром вводе в поле поиска, ответы на запросы приходят в неправильном порядке.

const SearchComponent: React.FC = () => {
  const [results, setResults] = useState<any[]>([]);

  const handleSearch = async (query: string) => {
    const data = await fetch(`/api/search?q=${query}`)
      .then(res => res.json());
    setResults(data);
  };

  return (
    <input 
      onChange={(e: ChangeEvent<HTMLInputElement>) => handleSearch(e.target.value)} 
    />
  );
};

Вопрос: Как исправить?

Ответ:

Отмена предыдущего запроса:

const SearchComponent: React.FC = () => {
  const [results, setResults] = useState<any[]>([]);
  const controllerRef = useRef<AbortController | null>(null);

  const handleSearch = async (query: string) => {
    controllerRef.current?.abort();
    controllerRef.current = new AbortController();
    
    try {
      const data = await fetch(`/api/search?q=${query}`, {
        signal: controllerRef.current.signal,
      }).then(res => res.json());
      
      setResults(data);
    } catch (err) {
      if ((err as Error).name !== 'AbortError') throw err;
    }
  };

  return (
    <input 
      onChange={(e: ChangeEvent<HTMLInputElement>) => handleSearch(e.target.value)} 
    />
  );
};

Дебаунс + проверка актуальности запроса:


  useEffect(() => {
    const timer = setTimeout(() => {
      if (input === lastQueryRef.current) return;
      lastQueryRef.current = input;
      handleSearch(input);
    }, 300);
    
    return () => clearTimeout(timer);
  }, [input]);
```

```ampcss
Приложение с ThemeContext ререндерит все компоненты при изменении темы, даже те, которые её не используют.

type ThemeContextType = {
  theme: string;
  setTheme: React.Dispatch<React.SetStateAction<string>>;
};

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

const App: React.FC = () => {
  const [theme, setTheme] = useState<string>('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <Content /> {/* Ререндерится даже если не зависит от theme */}
    </ThemeContext.Provider>
  );
};

Вопрос: Как оптимизировать?

Ответ:

Разделить value на два контекста:

const ThemeContext =  createContext<string| undefined>(undefined);
const ThemeUpdateContext = createContext<React.Dispatch<React.SetStateAction<string>>>();

value={{ theme, setTheme }} → 
<ThemeContext.Provider value={theme}>
  <ThemeUpdateContext.Provider value={setTheme}>

Для сложных объектов — мемоизировать value:

const contextValue = useMemo(() => ({ theme, setTheme }), [theme]);
```

```ampcss
//Палиндром на вход ф-я принимает строку возращает true/false является строка палиндромом или нет.
// Простое решение
function isPalindrom(str) {
    return str === str.split('').reverse().join('')
}

// попросить решить задачу другими вариантами, как вариант попробывать решить за log(n) через два указателя с начала и конца строки,
// каждую этерацию смещать каждый указатель на встречу друг другу.

function isPalindromLog(str) {
    //code
}
```

```ampcss
Дана скобочная последовательность, определить правильная она или нет?.

function isCorrectSequence(data: string): boolean {
}

// correct = "()"
// correct = "(()())"
// correct = "(()(()))"
// incorrect = ")()"
// incorrect = "(())("
// incorrect = "))(("
```

```ampcss
Задача: Написать в каком порядке будет выведен лог

console.log('Start');
setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');


Решение:

Start

End

Promise

Timeout
```
