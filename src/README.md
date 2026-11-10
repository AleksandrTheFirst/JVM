public class JvmComprehension { // создание описания класса в Metaspace

    public static void main(String[] args) { // создание фрейма в стеке
        int i = 1;                      // 1 Выделение памяти для примитива и присвоение значения в стеке (в хип ничего не попадает)
        Object o = new Object();        // 2 Выделение памяти и создание объекта в хип. Класс Object уже загружен в Bootstrap ClassLoader'oм.
        Integer ii = 2;                 // 3 Т.к. значение 2 попадает в область кешированных данных, новый объект не создается - возвращается ссылка на ранее созданный из хипа.
        printAll(o, i, ii);             // 4 В стеке потока main создается новый фрейм метода printAll
        System.out.println("finished"); // 7 В стеке потока main создается новый фрейм метода (sout), куда передается ссылка на объект "finished" 
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5 В хипе создается новый объект Integer (т.к. значение превышает кешированное) и присваивается значение. Класс Integer уже загружен в Bootstrap
        System.out.println(o.toString() + i + ii);  // 6 В стеке потока printAll создается новый фрейм метода (sout), куда передается ссылка на объект "o.toString() + i + ii"
    }
}