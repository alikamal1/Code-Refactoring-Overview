# Refactoring
# Code Smells
the smells are key signs that refactoring is necessary to enable further developement of the application with equal or greater speed.

# Bloaters
code, methods and classes that have increased to such gargantuan proportions that they are hard to work with.
## Log Method
method conatins too many lines of code (longer that ten lines)
- **Extract Method**
  ```PHP
  function printOwing() {
      $this->printBanner();
      print("name " . $this->name);
      print("amount" . $this->getOutstanding());
  }
  ```
  ```PHP
  function printOwing() {
      $this->printBanner();
      $this->printDetails($this->getOutstanding());
  }

  function printDetails($outstanding) {
      print("name " . $this->name);
      print("amount" . outstanding());
  }
  ```
- Reduce Local Variables and Parameters before extacting a Method using **Replace Temp with Query**
  ```PHP
  $basePrice = $this->quantity * $this->itemPrice;
  if($basePrice > 1000) {
      return $basePrice * 0.95;
  } else {
      return $basePrice * 0.98;
  }
  ```
  ```PHP
  if($this->basePrice() > 1000) {
      return $this->basePrice() * 0.95;
  } else {
      return $this->basePrice() * 0.98;
  }
  function basePrice() {
      return $this->quantity * $this->itemPrice;
  }
  ```
  **Introduce Parameter Object**
  ```PHP
  class Customer {
      amountInvoicedIn(Date $start, Date $end);
      amountReceivedIn(Date $start, Date $end);
      amountOverdueIn(Date $start, Date $end);
  }
  ```
  ```PHP
  class Customer {
      amountInvoicedIn(DateRange $date);
      amountReceivedIn(DateRange $date);
      amountOverdueIn(DateRange $date);
  }
  ```
  **Preserve Whole Object**
  ```PHP
  $low = $daysTempRange->getLow();
  $high = $daysTempRange->getHigh();
  $withinPlan = $plan->withinRange($low, $high);
  ```
  ```PHP
  $withinPlan - $plan->withinRnage($daysTempRange);
  ```
- **Replace Method with Method Object**
  ```PHP
  class Order {
      public function price() {
          $primaryBasePrice = 10;
          $secondaryBasePrice = 20;
          $TertiaryBasePrice = 30;
      }
  }
  ```
  ```PHP
  class Order {
      public function price() {
          return (new PriceCalculator($this))->compute();
      }
  }

  class PriceCalculator {
    private $primaryBasePrice;
    private $secondaryBasePrice;
    private $TertiaryBasePrice;
    public function __construct(Order $order) { }
    public function compute() { }
  }
  ```
- Conditionals and Loops are good clue that can can be mover to a separate method. **Decompose Conditional** for conditionals 
  ```PHP
  if($date->before(SUMMER_START) || $date->after(SUMMER_END)) {
      
  }
  ```

## Large Class
class contains many fields, methods or line of code

## Primitive Obsession
