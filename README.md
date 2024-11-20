# advanced-scope-exercise

```java

class ScopeChallenge {
    private int value = 1;
    private static int staticValue = 10;
    
    // Static inner class
    static class Config {
        private int value = 15;
        private static int staticValue = 20;
        
        static int getStaticValue() {
            return staticValue;  // Which staticValue?
            // Can we access ScopeChallenge.value here? Why?
        }
        
        int getValue() {
            return value + staticValue;  // Which staticValue?
            // Can we access ScopeChallenge.this.value here? Why?
        }
    }
    
    class Parent {
        protected int value = 2;
        
        Parent(int x) {
            setValue(x);  // Which setValue?
        }
        
        protected void setValue(int x) {
            value = x + 1;
            // Can we access Config.staticValue here? How?
        }
        
        int calculate(int x) {
            int value = 4;
            
            // Challenge 1: Understanding effectively final
            int multiplier = 3;
            // multiplier = 4; // What happens if we uncomment this?
            
            Runnable r = () -> {
                // Can we use 'value' here? Why?
                System.out.println(x * multiplier); // Why does this work?
                System.out.println(this.value);     // Which value?
            };
            
            return this.value + x;
        }
    }
    
    class Child extends Parent {
        private int value = 3;
        
        Child(int x) {
            super(x);
            setValue(x);
        }
        
        @Override
        protected void setValue(int x) {
            value = x + 2;
        }
        
        // Challenge 2: More effectively final examples
        void processValues() {
            int localValue = 25;
            // localValue++; // What happens if we uncomment this?
            
            class LocalClass {
                void print() {
                    System.out.println(localValue);  // Why does this work?
                    System.out.println(Child.this.value); // Which value?
                }
            }
            
            Consumer<Integer> consumer = (val) -> {
                System.out.println(val + localValue); // Why does this work?
                System.out.println(this.value);       // Which value?
            };
        }
        
        // Static nested class inside non-static class
        static class ChildConfig {
            void accessValues() {
                System.out.println(staticValue);  // Which staticValue?
                // Can we access Child.this.value here? Why?
                // Can we access Parent.this.value here? Why?
            }
        }
    }
    
    // Challenge 3: Test all concepts
    void test() {
        Config config = new Config();  // No need for 'new ScopeChallenge.Config()'
        Child.ChildConfig childConfig = new Child.ChildConfig();
        
        Parent p = new Child(5);
        Child c = new Child(5);
        
        int localValue = 30;
        // localValue = 31; // What happens if we uncomment this?
        
        Runnable r = () -> {
            System.out.println(localValue);  // Why does this work?
            System.out.println(this.value);  // Which value?
            // Can we access staticValue here? How?
        };
    }
}
