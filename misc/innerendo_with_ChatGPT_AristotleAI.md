			
			
◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻
*reference: copy paste from ChatGPT / Aristotle by Harmonic and edited, curated by Attila Vajda.*
not verified, non-machine checked, and maybe depends on basic axioms
◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻◇◻
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


			
>>
>>>
>>>>
>>>>>
>>>>>>



the verification pearl is 
ShiftInvariant + shiftInvariant_iff_orbitInvariant: 
	a predicate invariant under one multiplier step
	 is exactly a predicate constant on orbits, 
	 so any
		  σ_c-invariant property 
			  descends to the orbit space for free.
	
	RepunitOrder.lean 
	(namespace RepunitOrder): 
	the number-theory pearl 
	generalized to any base 
								b ≥ 2
								 — base_pow_eq_one 
								 (bⁿ = 1 in ZMod (bⁿ-1)),
								 
								  base_pow_ne_one 
								  (bᵏ ≠ 1 for 0 < k < n), 
								  
								  and 
								  
								  orderOf_base: 
									  
									  orderOf 
										  (b : ZMod (bⁿ-1)) = n.
									.
									.
									.
									.
									.
								equivariantEndo 
									= symmetry-respecting transformation, 
									
									.
									.
									.
								innerEndo 
									= mere change of coordinates, 
										
							and innerEndoIsoId 
									= proof that this change ofcoordinates 
									is invisible after passing to the 
									quotient/orbit level 
									— the same 
						“canonicalization modulo equivalence” idea 
						that drives the choice of simp normal forms. 🐚
			
			
			>
			>>
			>>>
			>>>>
			>>>>
			>>>>
			>>>>り
			
			    — inner symmetries are invisible on the orbit space π₀.

			the Frobenius endofunctor 
			(finite field of characteristic two):
			
			    frobAut: 
				    the squaring map x ↦ x² 
					    packaged as an additive automorphism.
					    
			    frobEndo: 
				    the genuine endofunctor 
					    of the conjugacy groupoid 
						    ActionCategory (AddAut F) (V → F) 
							    given by post-composition 
								    with Frobenius.
								
			    frobEndoIsoId: 
				    frobEndo ≅ 𝟭 
					    — the categorical statement that 
						    the Frobenius twist 
							    does not change the conjugacy class.
							    
			    duFunctor_obj_frobEndo: 
				    differential uniformity 
					    (
					    the invariant functor 
						    duFunctor from Conjugacy.lean
						) is unchanged by the Frobenius endofunctor.
			
			Shift dynamics on exponent space 
			("cyclotomic cosets as periodic orbits"):
			
			    frobAut_smul_pow: 
				    on monomials the endofunctor 
				    realises exponent doubling, 
				    
					    frobAut • (· ^ d) = · ^ (2 * d).
			    
			    expShift:
				     the doubling map d ↦ 2 * d on ZMod m, 
			
			and cyclotomicCoset: 
					its forward orbit.
			    
			expShift_bijective 
		and expShift_periodic: 
				when 2 is a unit modulo m, 
					the shift is a permutation 
				and every exponent is a periodic point, 
					so the cyclotomic cosets are 
						genuine periodic orbits.
