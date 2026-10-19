## Step 1 - Problem
### Online Shopping Website တစ်ခုတည်ဆောက်နေတယ်လို့ စဉ်းစားကြည့်ပါ။
### Team မှာ Developer သုံးယောက်ရှိတယ်။
-   ### Developer A -> Product Page
-   ### Developer B -> Shopping Cart
-   ### Developer C -> Payment System
### တစ်ယောက်ချင်းစီက Feature တွေကို Brach ခွဲပြီးရေးကြမယ်။ ပြီးရင် အားလုံးကို `main` Branch ထဲကို ပြန်ပေါင်းရမယ်။
### အဲ့မှာပြဿနာက Branch တွေကို ဘယ်လိုပြန်ပေါင်းမလည်းဆိုတာပါ။
## Step 2 - Lab Start
```bash
mkdir shopping-store-lab
cd shopping-store-lab
git init
git switch -c main
```
```bash
echo "Online Shopping Store v1" > shop.txt
git add shop.txt
git commit -m "Create shopping store project"
```
```bash
git log --oneline --graph --all
```
```bash
* 08190ba (HEAD -> main) Create shopping store project
```
## Step 3 - Fast-forward Merge
### Problem 
### Product Page ကို `feature-product` Branch မှာ Developer A က ရေးနေတယ်။
### `main` Branch မှာ တခြား Commit အသစ်မရှိသေးတဲ့အတွက် `feature-product` Branch ကို Fast-forward Merge နဲ့ တိုက်ရိုက်ရှေ့တိုးပေါင်းနိုင်တယ်။
### Lab 1 Fast-forward Merge
### 1.  `feature-product` Branch ကို ဖန်တီးပြီး အဲ့မှာ Add product page ဆိုပြီး commit တစ်ခုရိုက်လိုက်မယ်။
```bash
git switch -c feature-product
```
```bash
echo "Product page" >> shop.txt
git add shop.txt
git commit -m "Add product page"
```
```bash
git log --oneline --graph --all
```
```bash
* aae38f0 (HEAD -> feature-product) Add product page
* 08190ba (main) Create shopping store project
```
### 2.  `main` Branch ကိုပြန်သွားပြီးတော့ Merge လုပ်လိုက်မယ်။
```bash
git switch main
git merge feature-product
```
```bash
git log --oneline --graph --all
```
```bash
* aae38f0 (HEAD -> main, feature-product) Add productpage
* 08190ba Create shopping store project
```
### `main` နဲ့ `feature-product` က `Add product page` Commit တစ်ခုတည်းမှာ ရှိနေတာကိုတွေ့ရပါမယ်။
### ဘာကြောင့် `Fast-forward` ဖြစ်တာလည်း?
### `main` Branch မှာ သီးခြား Commit မရှိပဲ `feature-product` ကရှေ့ရောက်နေတာကြောင့်ပါ။ Git က Merge Commit အသစ် မဖန်တီးပဲ `main` Pointer ကို ရှေ့တိုးပေးလိုက်တဲ့ သဘောပါပဲ။
## Step 4 - Merge Commit
### Problem
### Developer B က Shopping Cart ဆိုတဲ့ feature ကို `feature-cart` branch မှာရေးနေတုန်း
### `main` မှာ Commit အသစ်ရှိလာတယ်ဆိုပါစို့
### ဆိုလိုတာက ဒီလိုအခြေအနေမှာ Branch နှစ်ခုလုံးမှာ သီးခြား Commit တွေနဲ့ ဖြစ်နေလို့ Merge Commit ကို အသုံးပြုကြည့်မယ်။ 
### Lab 2 - Merge Commit
### 1. Developer B က `feature-cart` Branch ကို ဖန်တီးပြီးတော့ `feature` တွေရေးမယ်။
```bash
git switch -c feature-cart
```
```bash
echo "Shopping cart" >> shop.txt
git add shop.txt
git commit -m "Adding shopping cart"
```
```bash
git log --oneline --graph --all
```
```bash
* 3490eed (HEAD -> feature-cart) Adding shopping cart
* 277ccc0 (main, feature-product) Add product page
* d37ebdd Create shopping store project
```
### 2.    `main` ကိုပြန်သွားပြီး commit အသစ်တစ်ခုထည့်မယ်။
```bash
git switch main
echo "Store homepage" >> shop.txt
git add shop.txt
git commit -m "Add store homepage"
```
```bash
git log --oneline --graph --all
```
```bash
* 24fb82e (HEAD -> main) Add store homepage
| * 50e5e3b (feature-cart) Adding shopping cart
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
### အခုဆိုရင် `main` နဲ့ `feature-cart` တွေမှာ မတူညီတဲ့ Commit တွေမှာရှိနေပါပြီ။
### 3.  Merge Commit ကိုဖန်တီးပါ။
```bash
git merge --no-ff feature-cart -m "Merge shopping cart"
```
```bash
Auto-merging shop.txt
CONFLICT (content): Merge conflict in shop.txt
Automatic merge failed; fix conflicts and then committhe result.
```
## Step 5 - Resolve Merge Conflict
### `main` နဲ့ `feature-cart` Branch နှစ်ခုလုံးက shop.txt ရဲ့ တူညီတဲ့နေရာမှာ  ပြောင်းလဲမှုလုပ်ထားလို့ git က Auto-merging လုပ်မရပါဘူး။
### 1.  `shop.txt` ကိုဖွင့်ရင် ခုလိုမျိုး conflit ဖြစ်နေတာကို တွေနိုင်ပါတယ်။
```bash
Online Shopping Store v1
Product page
<<<<<<< HEAD
Store homepage
=======
Shopping cart
>>>>>>> feature-cart

```
### 2.  Confilict ဖြစ်နေတာတွေကို ပြင်ပြီး save လုပ်လိုက်ပါ။
```bash
Online Shopping Store v1
Product page
Store homepage
Shopping cart
```
### 3.  ပြင်ဆင်ပြီးသွားရင် Merge ကို အပြီးသတ်ပါ။
```bash
git add shop.txt
git commit -m "Resolve shopping cart merge confilict"
```
```bash
git log --oneline --graph --all
```
### အခုဆိုရင် Branch နှစ်ခုရဲ့ History ကို Merge Commit နဲ့ ပြန်ချိတ်ထားတာကို မြင်ရပါမယ်။
```bash
*   f7730a5 (HEAD -> main) Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
## Step 6 - Squash Merge
### Problem
### ဥပမာ Shopping Cart ကိုရေးတဲ့အခါ Commit ၂ ခု လုပ်ထားတယ်ဆိုပါစို့။
-   ### Add checkout page
-   ### Add order confirmation page
### ဒီ Commit ၂ ခုကို `main` ထဲပေါင်းတဲ့အခါ Commit တစ်ခုတည်းဖြစ်အောင် စုစည်းလိုက်တာက Squash Merge ပါ။
### Lab 3 - Squash Merge
### 1.  `feature-checkout` ကို ဖန်တီးပြီး Commit နှစ်ခု ဖန်တီးလိုက်ပါမယ်။
```bash
git switch -c feature-checkout
```
```bash
echo "Checkout page" >> shop.txt
git add shop.txt
git commit -m "Add checkout page"
```
```bash
git log --oneline --graph --all
```
```bash
* 10384a2 (HEAD -> feature-checkout) Add checkout page
*   f7730a5 (main) Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
```bash
echo "Order confirmation page" >> shop.txt
git add shop.txt
git commit -m "Add order confirmation page"
```
```bash
git log --oneline --graph --all
```
```bash
* c6e3a06 (HEAD -> feature-checkout) Add order confirmation page
* 10384a2 Add checkout page
*   f7730a5 (main) Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
### 2. `main` ထဲပြန်သွားပြီး Squash Merge လုပ်ပါ။
```bash
git switch main
git merge --squash feature-checkout
git commit -m "Add checkout feature"
```
```bash
git log --oneline --graph --all
```
```bash
* 1575dc5 (HEAD -> main) Add checkout feature
| * c6e3a06 (feature-checkout) Add order confirmation
| * 10384a2 Add checkout page
|/  
*   f7730a5 Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
### ဒီနည်းနဲ့ `feature-checkout` Branch ရဲ့ ပြောင်းလဲမှုတွေကို `main` Branch ထဲမှာ `Add checkout feature` ဆိုတဲ့ Commit တစ်ခုတည်းအဖြစ်သိမ်းလိုက်တာပါ။
### `feature-checkout` Branch မှာ မူရင်း Commit နှစ်ခုရှိသေးပေမဲ့ `main` Branch မှာ Checkout အတွက် Commit တစ်ခုတည်း ဖြစ်နေတာကို သတိထားကြည့်ပါ။
## Step 7 - Rebase
### Problem
### ဥပမာ Developer C က `feature-payment` မှာ Payment System ကိုရေးတုန်း `main` Branch မှာ Feature အသစ်တစ်ခု ထပ်ထည့်လိုက်တယ်ဆိုပါစို့။
### Rebase လုပ်တယ်ဆိုတာက `feature-payment` Branch ရဲ့ Commit ကို `main` Branch ရဲ့ နောက်ဆုံး Commit အပေါ်ကို ပြန်တင်ပေးတာပါ။
### Lab 4 - Rebase 
### 1.  `feature-payment` Branch ဖန်တီးပြီးတော့ Payment Feature တွေကို ရေးပြီးသွားရင် `commit` လုပ်လိုက်ပါမယ်။
```bash
git switch -c feature-payment
```
```bash
echo "Payment system" >> shop.txt
git add shop.txt
git commit -m "Add payment system"
```
```bash
git log --oneline --graph --all
```
```bash
* 71248c5 (HEAD -> feature-payment) Add payment system
* 1575dc5 (main) Add checkout feature
| * c6e3a06 (feature-checkout) Add order confirmation
| * 10384a2 Add checkout page
|/  
*   f7730a5 Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
### 2.  `main` Branch မှာ လည်း Commit အသစ် တစ်ခု ဖန်တီးလိုက်ပါမယ်။
```bash
git switch main
echo "Customer profile" >> shop.txt
git add shop.txt
git commit -m "Add customer profile"
```
```bash
git log --oneline --graph --all
```
```bash
* 82b9b21 (HEAD -> main) Add customer profile
| * 71248c5 (feature-payment) Add payment system
|/  
* 1575dc5 Add checkout feature
| * c6e3a06 (feature-checkout) Add order confirmation
| * 10384a2 Add checkout page
|/  
*   f7730a5 Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
### အခုဆိုရင် `main` နဲ့ `feature-payment` မှာ မတူညီတဲ့ commit တွမှာရှိနေပါပြီ။
### 4.  Rebase လုပ်ကြည်ပါမယ်။
```bash
git switch feature-payment
git rebase main
```
```bash
Switched to branch 'feature-payment'
Auto-merging shop.txt
CONFLICT (content): Merge conflict in shop.txt
error: could not apply 71248c5... Add payment system
```
## Step 8 - Resolve Rebase Conflit
### ဒီတစ်ခါလည်း `main` နဲ့ `feature-payment` Branch နှစ်ခုလုံးက Commit တွေမှာ `shop.txt` ကို နေရာတစ်ခုတည်းမှာ ပြင်ဆင်မိထားလို့ merge conflict တက်သွားတာပါ။
```bash
Online Shopping Store v1
Product page
Store homepage
Shopping cart

Checkout page
Order confirmation page
<<<<<<< HEAD
Customer profile
=======
Payment system
>>>>>>> 71248c5 (Add payment system)
```
### `conflict` ဖြစ်နေတာတွေကိုပြင်ပြီးသွားရင် `rebase --continue` ဆက်လုပ်လိုက်ပါမယ်။
```bash
Online Shopping Store v1
Product page
Store homepage
Shopping cart
Checkout page
Order confirmation page
Customer profile
Payment system
```
```bash
git add shop.txt
git rebase --continue
``` 
```bash
git log --oneline --graph --all
```
```bash
* 72190a2 (HEAD -> feature-payment) Add payment system
* 82b9b21 (main) Add customer profile
* 1575dc5 Add checkout feature
| * c6e3a06 (feature-checkout) Add order confirmation
| * 10384a2 Add checkout page
|/  
*   f7730a5 Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```
```bash
git switch main
git merge feature-payment
```
```bash
git log --oneline --graph --all
```
```bash
* 72190a2 (HEAD -> main, feature-payment) Add payment system
* 82b9b21 Add customer profile
* 1575dc5 Add checkout feature
| * c6e3a06 (feature-checkout) Add order confirmation
| * 10384a2 Add checkout page
|/  
*   f7730a5 Resolve shopping cart merge confilict
|\  
| * 50e5e3b (feature-cart) Adding shopping cart
* | 24fb82e Add store homepage
|/  
* aae38f0 (feature-product) Add product page
* 08190ba Create shopping store project
```