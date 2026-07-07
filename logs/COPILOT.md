**אני:**
אני צריך לכתוב בדיקות יחידה לפונקציה `isPrime` מתוך הקובץ `App.java` שלי. תוכל להראות לי איך לכתוב טסטים שמכסים נכון את הלוגיקה וגם מקרי קצה?

**AI:**
בטח. פונקציה שבודקת מספרים ראשוניים צריכה לוודא שמספרים כמו 2 ו-17 מחזירים אמת, אבל חשוב מאוד לבדוק גם מקרי קצה כמו מספרים שליליים, 0 ו-1 שאמורים להחזיר שקר. הנה דוגמה לטסט כזה באמצעות פונקציות Assert של JUnit:

` ` `java
    @Test
    public void testIsPrime() {
        assertTrue(App.isPrime(2));
        assertTrue(App.isPrime(17));
        assertFalse(App.isPrime(1)); // מקרה קצה (קטן מ-2)
        assertFalse(App.isPrime(15));
        assertFalse(App.isPrime(-5)); // מספר שלילי
    }
` ` `

**אני:**
הבנתי את העיקרון עם `assertTrue` ו-`assertFalse`. אבל לגבי הפונקציה `factorial` אצלי בקוד, היא זורקת שגיאת `IllegalArgumentException` אם המספר הוא שלילי. איך אני בודק ב-JUnit שהשגיאה הזו באמת נזרקת?

**AI:**
שאלה מצוינת. כדי לבדוק זריקת חריגים, JUnit מספקת את הפונקציה `assertThrows`. היא בודקת שהקוד בתוכה אכן קורס עם השגיאה המצופה. ככה זה ייראה עבור פונקציית העצרת שלך:

` ` `java
    @Test
    public void testFactorial() {
        assertEquals(120, App.factorial(5));
        assertEquals(1, App.factorial(0)); // מקרה קצה - עצרת של 0 היא 1
        
        // בדיקת זריקת שגיאה על מספר שלילי
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            App.factorial(-1);
        });
        assertEquals("Negative number", exception.getMessage());
    }
` ` `

**אני:**
מעולה, זה סוגר לי את הפינה של הנתיבים השונים בפונקציות. אתה יכול בבקשה להשלים לי את מחלקת ה-`AppTest.java` עם שאר הבדיקות לפונקציות בקובץ (כמו `isPalindrome`, `fibonacciUpTo`, `average` וכו') תוך שימוש ב-Asserts שונים כדי שיהיה לי 100% כיסוי קוד?

**AI:**
בוודאי, הנה המחלקה המלאה שמשלימה את כל הבדיקות עבור הפונקציות שלך, מכסה את כל משפטי התנאי ומקרי הקצה:

` ` `java
package org.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.*;

public class AppTest {

    @Test
    public void testAdd() {
        assertEquals(5, App.add(2, 3));
        assertEquals(-1, App.add(-4, 3));
        assertEquals(0, App.add(0, 0));
    }

    @Test
    public void testIsPrime() {
        assertTrue(App.isPrime(2));
        assertTrue(App.isPrime(17));
        assertFalse(App.isPrime(1));
        assertFalse(App.isPrime(15));
        assertFalse(App.isPrime(-5));
    }

    @Test
    public void testReverse() {
        assertEquals("cba", App.reverse("abc"));
        assertEquals("", App.reverse(""));
        assertEquals("a", App.reverse("a"));
    }

    @Test
    public void testFactorial() {
        assertEquals(120, App.factorial(5));
        assertEquals(1, App.factorial(0));
        
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            App.factorial(-1);
        });
        assertEquals("Negative number", exception.getMessage());
    }

    @Test
    public void testIsPalindrome() {
        assertTrue(App.isPalindrome("Racecar"));
        assertTrue(App.isPalindrome("A man, a plan, a canal, Panama"));
        assertFalse(App.isPalindrome("hello"));
    }

    @Test
    public void testFibonacciUpTo() {
        List<Integer> expected = Arrays.asList(0, 1, 1, 2, 3, 5, 8);
        assertEquals(expected, App.fibonacciUpTo(10));
        assertEquals(Arrays.asList(0), App.fibonacciUpTo(0));

        assertThrows(IllegalArgumentException.class, () -> {
            App.fibonacciUpTo(-5);
        });
    }

    @Test
    public void testCharFrequency() {
        Map<Character, Integer> expected = new HashMap<>();
        expected.put('a', 2);
        expected.put('b', 1);
        assertEquals(expected, App.charFrequency("aab"));
        assertTrue(App.charFrequency("").isEmpty());
    }

    @Test
    public void testIsAnagram() {
        assertTrue(App.isAnagram("Listen", "Silent"));
        assertTrue(App.isAnagram("a gentleman", "elegant man"));
        assertFalse(App.isAnagram("hello", "world"));
    }

    @Test
    public void testAverage() {
        int[] arr = {2, 4, 6};
        assertEquals(4.0, App.average(arr), 0.001);

        assertThrows(IllegalArgumentException.class, () -> {
            App.average(new int[]{});
        });
    }

    @Test
    public void testFilterEvens() {
        List<Integer> input = Arrays.asList(1, 2, 3, 4, 5, 6);
        List<Integer> expected = Arrays.asList(2, 4, 6);
        assertEquals(expected, App.filterEvens(input));
        assertTrue(App.filterEvens(Arrays.asList(1, 3, 5)).isEmpty());
    }

    @Test
    public void testMostCommonWord() {
        String text = "Hello world! Hello everyone.";
        assertEquals("hello", App.mostCommonWord(text));
    }
}
` ` `