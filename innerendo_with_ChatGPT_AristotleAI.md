			
	reference: copy paste from ChatGPT / Aristotle by Harmonic and edited, curated by Attila Vajda.
	>
	>
	>
	>


>
>
>
>



			Categorical scaffolding (general):
			- `equivariantEndo`: 
				a monoid endomorphism
					 `φ : G →* G` together with a 
				
				`φ`-equivariant self-map 
					`F` of `X` (`F (g • x) = φ g • F x`) yields 
					
				an endofunctor of the action groupoid 
					`ActionCategory G X`.
			
			- `innerEndo h`: 
				the special inner case, 
				translation by a fixed
					 `h : G`.
			
			- `innerEndoIsoId h`: 
				the inner endofunctor is 
				naturally isomorphic to the identity 
					— inner symmetries are invisible 
						on the orbit space π₀.
					
					
					>
					>
					>
					>
					>
					
					
					



		equivariantEndo
		      ↓
		endofunctor on ActionCategory G X



					is a constructor: 
						given data (φ, F), 
							build a functor.
					
					>
					like
					ExistsUnique.intro
					(P → Q) ∧ (¬P → R)
					is goal-facing: 
					assemble structure from obligations.
				.
				.
				.
				.
				
				
						By contrast,
		
		innerEndoIsoId h
		
		is an eliminator/use theorem:
		
								innerEndo h  ≅  Id
		
				It lets you consume 
				an inner symmetry and 
				replace it by identity 
				up to isomorphism.

>
>>
>>>


					Like:

					rcases h with ...

							or

									h : (P ∧ Q) ∨ (¬P ∧ R)

					it is hypothesis-facing: 
					extract consequences.


					
?
?
?
?🌿
```

BUILD side                    USE side

equivariantEndo          innerEndoIsoId
ExistsUnique.intro       rcases h
ite_prop_iff_and         ite_prop_iff_or
```


<

<
<
<
<

a quotient-level statement:

					inner symmetry
					      ≅ Id
					        ↓
					acts trivially on π₀

		so "inner structure disappears 
		after decategorifying 
		to connected components/orbits." 🐚


	🌳 Category-theoretically:

		constructors 
			= exhibit a universal object/morphism
		eliminators 
			= exploit a universal property / isomorphism