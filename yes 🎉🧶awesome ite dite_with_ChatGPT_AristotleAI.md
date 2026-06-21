

🌳

**\_and to prove it, \_or to use it**  
*goal position vs hyp pos*  
 "put each form where every rule firing is invertible."  

>
>
>
>
*to prove:*
```lean
⊢ if P then Q else R
```
rewriting to
```lean
⊢ (P → Q) ∧ (¬P → R)
```
is great:   
	`constructor`; then assume `P` or `¬P`.  

>
>
>
>

*to use:*
```lean
h : if P then Q else R
```
rewriting `h` to
```lean
h : (P ∧ Q) ∨ (¬P ∧ R)
```
>
>
>
>

`cases h` gives branch condition 
	and result together.


* goal/prove ⇒ `_and` is ergonomic
* hypothesis/use ⇒ `_or` is ergonomic


>
>
>
>>
>
>
>

right-rules for ∧ are invertible;  
left-rules for ∨ are informative. 🐚  


<div class="spacer"><br></div>

>
>
>
>>
>
>
>

🌊flow is:

```
goal: (P→Q) ∧ (¬P→R)
  constructor
    goal1: P→Q   → intro P
    goal2: ¬P→R  → intro ¬P
```

<div class="spacer"><br></div>

>
>
>
>>
>
>
>


key idea 🐚:

constructor = split conjunction into independent obligations  
intro P / intro ¬P = move assumptions into context  

		
		No branching yet; 		
		
		just structural decomposition 		
		
		of proof obligations 		
		
		(like α-rules in tableaux).		

>
>
>
>>
>
>
>

\_or:  

(P ∧ Q) ∨ (¬P ∧ R)  

you cannot use constructor,  
∵ there isn't a deterministic split  


<div class="spacer"><br></div>

you have to choose a side:

```lean id="orflow2"
left 🌿
⊢ P ∧ Q
  constructor
    ⊢ P
    ⊢ Q
```

```lean id="orflow3"
right 🌿
⊢ ¬P ∧ R
  constructor
    ⊢ ¬P
    ⊢ R
```

 then flow is:
```
goal: (P∧Q) ∨ (¬P∧R)
  left OR right   ← NON-DETERMINISTIC choice
    constructor
      split again
```
>
>
>
>>
>
>
>

<div class="spacer"><br></div>
🌳

* `_and` form:   
	**no choice**, only decomposition  
* `_or` form:   
	**must commit first**, then decompose  

		this why `simp` prefers   
			`_and` version:   
				which avoids introducing   
					a *search decision* too early.  
			
  <div class="spacer"><br></div>

* `∧` = “I owe you both proofs”   
	(parallel obligations)  
* `∨` = “I claim one of two worlds is true”   
	(must pick world first)  

			in tableaux:		

			* ∧ = α-rule 		
				(no branching)		
			* ∨ = β-rule 		
				(branching search node)		

			`_or` is inherently a **choice point**, 		
				not a pure structural flow. 🐙		

<div class="spacer"><br></div>

>
>
>
>>
>
>
>


split shows up at use-time, in Rewrite.lean:   
when preprocess turns each ↔ into one Eq rewrite,   

		both \_and and \_or get keyed under ite. 

			both become valid candidates for the same LHS. 
			 difference becomes meaningful when tryTheoremCore rewrites: 
			 
				 _and yields (P→Q)∧(¬P→R)
					  (deterministic, simp-friendly);
					  
				_or yields a disjunction simp won't usefully drive. 
				
				Having both @[simp]
					 = same key, 
					 two RHS = non-confluent.

the issue is not definition-time but use-time normalization.  


>
>
>
>>
>
>
>

implication between the two formulations:  

Let  

		\_or  := (P ∧ Q) ∨ (¬P ∧ R)  

		\_and := (P → Q) ∧ (¬P → R)  


	Then:

	### \_or → \_and ✔ constructive


		because if you know

		(P ∧ Q) ∨ (¬P ∧ R)
			
			then:

				in the left case, P→Q is easy (you have Q)
				in the right case, ¬P→R is easy (you have R)

					No EM needed.


>
>
>
>>
>
>
>

	### `_and → _or` ✘ generally not constructive

from
```lean id="a9w6tx"
(P → Q) ∧ (¬P → R)
```

you only have **instructions**:
```lean id="vgvyq4"
if P then Q
if ¬P then R
```

to produce
```lean id="rck4l9"
(P ∧ Q) ∨ (¬P ∧ R)
```

		you must know  
			whether `P` or `¬P` holds.  

which requires:  

$$
P \lor \neg P
$$

		(decidability / excluded middle).  

so:  
```text id="1zlkjh"
case information  ⇒ instructions
instructions      ⇒ case information   (needs EM)
```

>
>
>
>>
>
>
>

🐚 data ⇒ rules is easy.  

		rules ⇒ data 
			requires deciding which world you're in.

>
>
>
>>
>
>
>


the polarity is the same fact viewed three ways: 

			negative/invertible ≈ choiceless/product ≈ normalization-friendly, 
			
				versus 
			
					positive/case-friendly ≈ sum/commitment. 
			
			simp lives on the left column; 
			rcases/by_cases live on the right. 🌳

>
>
>
>>
>
>
>


dite_prop_iff_or: 

		dite P Q R ↔ (∃ h, Q h) ∨ (∃ h, R h). 
		
		it's sum + existential: 
			
				∨ forces committing to a disjunct,
				 and ∃ h, … forces producing a witness 
					 (a proof of P or ¬P) 
						 — exactly a decision/case-split on P. 
						 
				Positive/commitment, right column. 
				Good for rcases, not for a normalizer.


>
>
>
>>
>
>
>
dite_prop_iff_and: 

		dite P Q R ↔ (∀ h, Q h) ∧ (∀ h, R h). 
		
			∧ is still a choiceless product,
			 and ∀ h : P, … / ∀ h : ¬P, … 
				 are the dependent analogues of P → … / ¬P → … 
				 
				 — you assume the (dis)proof of P 
				 and discharge each side independently. 
				 
			
			Negative/invertible/normalization-friendly, 
			same left column.
				 simp can drive it deterministically.


>
>
>
>>
>
>
>

	 → ⇝ ∀ and ∧
		  stay choiceless 
			  (simp-friendly); 
			  
	∨ ⇝ ∃-witness 
		stay commitment-heavy 
			(search). 
			
			
		Same polarity, 
			lifted to the dependent setting. 🌳


>
>
>
>>
>
>
>


in choosing whether to add`@[simp]` to \_and or \_or,  
the difference seems to be in how `simp` processes each  

and `simp`  

>
>
>
>>
>
>
>

	an ∨ goal forces you to pick a branch
		—simp won't decide P for you,
		 so it can't make progress on it. 
		 
	the \_and form (P→Q)∧(¬P→R) 
		splits deterministically into two subgoals 
			(and simp +contextual even assumes P/¬P). 
			
			so \_or is awkward in goal position 
			but fine for consuming a hypothesis: 
			
				rcases (ite_prop_iff_or.mp h).


>
>
>
>>
>
>
>

\_or is handy only for consuming an ite in a hypothesis.
\_or form gives (P∧Q)∨(¬P∧R), 
	
			where simp/constructor can't pick a disjunct; 
			
			you'd need rcases and a decision.


>
>
>
>>
>
>
>

— they're De Morgan duals as logic,   
but simp is not symmetric in them,   

because simp rewrites a goal   
and needs a deterministic target.  


	    An ∧ on the RHS 
		    ((P→Q) ∧ (¬P→R))
			     is a product: 
			     
			     a single constructor. 
			     
			to prove it simp just splits into two independent subgoals 
			— choiceless, confluent. 
			
			
			so the _and form is a good simp normal form.

>
>
>
>>
>
>
>
		
		An ∨ on the RHS 
			((P∧Q) ∨ (¬P∧R))
				 is a sum:
				 
				two constructors. 
				
			to prove it something must choose a disjunct, 
			i.e. decide P. simp won't make that choice, 
			
			
			so the _or form is bad as a goal-rewrite.

>
>
>
>>
>
>
>

also, \_or → \_and is intuitionistic,  
but _and → \_or needs Decidable P/excluded middle.  
```
	so _or is logically stronger, 
	not a mirror image; 
	_and is the constructive floor.
```
>
>
>
>
so: 

		dual as propositions, 
			asymmetric operationally 
				— simp favors the product (_and) 
					and balks at the sum (_or). 
					
			The _or form is better 
				for consuming an ite in a hypothesis (rcases), 
					not for goals.

>
>
>
>

they are dual as propositions,   
but asymmetric operationally,  
because   


>
>
>
>



its about invertibility:

	    ∧R invertible, →R invertible, ∀R invertible
	    ∨L invertible, ∧L invertible, ∃L invertible
	    
	    ∨R not invertible (must pick a disjunct)
	    →L not invertible (must discharge the antecedent)

so use each form on the side 
		where all of its connectives have invertible rules.	


>
>
>
>>
>
>
>

the tableaux view predicts correctly: 
	
			the ∨R choice point on Prop 
				is literally deciding P. 
				
			it's why _or → _and 
				is intuitionistic 
			but _and → _or needs Decidable P/em 
			
			— the non-invertible right rule for ∨ 
				is exactly where excluded middle has to enter. 
				
			so _and is also the constructive floor,
				 which is the third independent reason 
					 it's the safe global normal form.

>
>
>
>>
>
>
>
Modern name for the dichotomy: 
	
		this is focusing / polarity (Andreoli). 

	 - invertible/asynchronous connectives 
		 (∧, →, ∀) 
			 polarize negatively 
				 and are decomposed eagerly 
					 toward a goal;
		
	-  synchronous ones 
		(∨, ∃) 
			polarize positively 
			and carry the real "data"/choices, 
				naturally consumed from a hypothesis. 
				
					_and is the negative packaging of ite, 
					_or the positive one.

>
>
>
>

∧R = product of obligations  
∨L = coproduct of cases  

This is exactly product vs coproduct in category theory.

>
>
>
>

4. Tiny structural picture 🌳
```
∧R
goal: A ∧ B
   ↓
prove A   prove B
(no tree growth)


∨L
h : A ∨ B
   ↓
case A      case B
```
>
>
>
>

6. Tableaux intuition 🐚  
∧R (goal) = no branching  
∨L (hyp) = branching but informed  
∨R (goal) = branching guess  
∧L (hyp) = weak info, no cases  



			_and avoids search; 
			_or reveals cases; 
			swapping them moves branching
				 from “known” to “unknown”. 🌿
>
>
>
>




>>
>
>
>

(negative/invertible packaging 
		⇒ good goal normal form ⇒ constructive floor)


mentally treat them as one unit,  
since splitting them leaves dependent dite goals   
		in an inconsistent state.



P → Q   ↦   ∀ h : P, Q h  


⊢ A → B ⇔ assume A, prove B  



	dite P Q R
	-- Q : P → Prop
	-- R : ¬P → Prop



	goal: ∀(h:P), Q h   ∧   ∀(h:¬P), R h
	        ↓
	intro hP     intro hNP
	prove Q hP   prove R hNP
	(no tree growth)

>
>
>
>


		h : dite P Q R
		        ↓
		cases h

		gives:

		case 1: hP : P
		        hQ : Q hP

		case 2: hNP : ¬P
		        hR : R hNP



```
           dite P Q R
               |
        ┌──────┴──────┐
        P             ¬P
        |              |
      Q(P)           R(¬P)
```

>
>
>
>>
>
>
>



 — here are all **4 in the same structural style** 🌳🐚

>
>
>
>

 1. `∧R` (goal: A ∧ B)

```text
goal: A ∧ B
      |
   ┌──┴──┐
   A     B
   |     |
 prove  prove
```

(no branching, just two obligations)

>
>
>
>

2. `∨L` (hyp: A ∨ B)

```text
h : A ∨ B
      |
   ┌──┴──┐
   A     B
   |     |
 case   case
```

(branching analysis of given info)

>
>
>
>
 3. `ite` (non-dependent)

```text
ite P Q R
      |
   ┌──┴──┐
   P     ¬P
   |     |
   Q     R
```

⚠️ but: branches do NOT receive P / ¬P as usable data

>
>
>
>

 4. `dite` (dependent)

```text
dite P Q R
      |
   ┌──┴──┐
   P     ¬P
   |     |
 Q(P)   R(¬P)
```

✔ branches *carry the witness / proof*  
✔ fully information-aware branching  



>
>
>
>

---

 5. Big structural takeaway 🌿

| form | structure type     | info flow          |
| ---- | ------------------ | ------------------ |
| ∧R   | split obligations  | no branches        |
| ∨L   | case split         | unknown → revealed |
| ite  | blind branching    | no dependency      |
| dite | informed branching | dependency-aware   |

>
>
>
>





— they are the **same core 2×2 geometry**, 
just with dependency added in the `dite` row. 🌳🐚    
Here are the structural drawings in one unified style:  


>
>
>
>



		✔ invertible (safe, no choice)

		Applying them never requires guessing.

		∧R, →R, ∀R (goal side)
		∨L, ∃L (hyp side)



		✖ non-invertible (choice points)

		They require commitment:

		∨R (choose left/right in goal)
		∃R (choose witness)
>
>
>
>




2. Why _and aligns with invertible goal rules  
		Your \_and shape:
		(P → Q) ∧ (¬P → R)

		uses:
		∧R → split goal
		→R → intro assumptions

		Both are invertible:
			
			goal splits 
				→ no branching 
					→ deterministic descent

		So simp is happy: 
			it never has to guess.


>
>
>
>



 0. The four lemmas (the 2×2 picture)
`Mathlib/Logic/Basic.lean`:
```lean
theorem ite_prop_iff_or  : (if P then Q else R) ↔ (P ∧ Q) ∨ (¬P ∧ R)
theorem ite_prop_iff_and : (if P then Q else R) ↔ (P → Q) ∧ (¬P → R)
theorem dite_prop_iff_or  {Q : P → Prop} {R : ¬P → Prop} :
    dite P Q R ↔ (∃ p, Q p) ∨ (∃ p, R p)
theorem dite_prop_iff_and {Q : P → Prop} {R : ¬P → Prop} :
    dite P Q R ↔ (∀ h, Q h) ∧ (∀ h, R h)
```


 form a clean 2×2 lattice:

|            | **`_or` column** (disjunction) | **`_and` column** (conjunction) |
| ---------- | ------------------------------ | ------------------------------- |
| **`ite`**  | `(P∧Q) ∨ (¬P∧R)`               | `(P→Q) ∧ (¬P→R)`                |
| **`dite`** | `(∃ p, Q p) ∨ (∃ p, R p)`      | `(∀ h, Q h) ∧ (∀ h, R h)`       |

two axes of duality run through this table
				— and understanding them *is* the  
					answer to the question.  
			

		* **Rows (ite ↦ dite):** 
				the non-dependent row 
					is the *propositional shadow* of
				  the dependent row. `(P → Q)` 
					  is exactly 
					  
					* `∀ _ : P, Q` and `(P ∧ Q)` is exactly
		  
					* `∃ _ : P, Q` (`exists_prop`).

				so `dite` is the honest statement and `ite` is
			  what you get by forgetting that the branch may use the proof.
			

		* **Columns (`_or` ↦ `_and`):** 
		* 
				the two columns are **De Morgan / polarity
					  duals**, swapped by negation (Fact 4). 
						  
						  `_or` is built from `∨`/`∃` 
							  (positive, *existential*),
							  
						`_and` from `∧`/`∀`/`→` 
								(negative, *universal*).
							
Everything else is a consequence of these two axes.

>
>
>
>

simp has no canonical normal form. 
	
	Its output depends entirely
	 on the active rewrite set 
		
		 (which @[simp] lemmas, 
		 simp only [...], 
		 priorities, 
		 direction, 
		 dischargers, 
		 congruence rules). 
		 
	 It's a terminating-ish rewrite strategy: 
	 repeatedly apply matching oriented rules bottom-up 
	 until none fire. 
	 
		 Change the rule set and the "normal form" changes; 
		 
		 non-confluent sets can even give 
			 order-dependent results. 
		 
		 So: a normal form relative to a chosen rewrite system, 
	not an absolute canonical one.




	>
	>
	>




7. The key conceptual correction 🧠

		Not:
			simp produces canonical normal form

		but:
			simp produces a strategy-dependent 
				normal form under a chosen rewrite system




>
>
>



		8. One-line essence 🌿

		simp is a proof-carrying, 
		bottom-up conditional 
		rewrite engine whose 
		notion of “normal form”
		is determined entirely 
		by the invertibility and 
		termination properties of 
		the selected `@[simp]` rules.

