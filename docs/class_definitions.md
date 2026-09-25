# Class definitions and label rules

**Label-rule review and dataset spot-check:** Rama Abu Ghoush (Group 7)

Five classes. The upstream dataset (Construction Site Safety, Roboflow Universe) was labelled by other people, so these rules serve two purposes: they are how we label any images we add ourselves, and they are the yardstick we used when spot-checking the inherited labels.

The design choice worth explaining is the pair of "NO-" classes. A detector only finds things that are present in the image, so it cannot directly report that a hardhat is *absent*. Labelling the bare head (`NO-Hardhat`) and the uncovered torso (`NO-Safety Vest`) turns a missing item into something visible that the model can learn. The cost is that these classes are harder: they depend on the model noticing what is *not* there.

## Hardhat
- **What:** a rigid safety helmet being worn on a person's head.
- **Box:** tight around the helmet itself, not the whole head.
- **Include:** any colour; helmets partly hidden by a hood or ear defenders if the shell is visible.
- **Exclude:** helmets that are hanging from a belt, lying on the ground or on a shelf (not being worn); bicycle and motorcycle helmets.

## NO-Hardhat
- **What:** a person's head that is visible but has no safety helmet on it.
- **Box:** tight around the head, including hair.
- **Include:** bare heads, baseball caps, beanies, hoods, headscarves — none of these are safety helmets.
- **Exclude:** heads too small or blurred to judge (under about 15 px tall at 640 px) — leave unlabelled rather than guess; heads facing fully away where the top of the head cannot be seen.

## Safety Vest
- **What:** a high-visibility vest or jacket (fluorescent yellow/orange/green with reflective bands) being worn.
- **Box:** around the garment on the torso.
- **Include:** hi-vis jackets and coats, not only sleeveless vests; vests partly covered by a harness.
- **Exclude:** ordinary coloured clothing that happens to be yellow or orange without reflective banding; vests being carried or hung up.

## NO-Safety Vest
- **What:** a person's torso that is visible and has no high-visibility garment on it.
- **Box:** shoulders to waist.
- **Exclude:** torsos mostly hidden behind equipment or other people (more than about half occluded) — leave unlabelled.

## Person
- **What:** every person in the image, whatever PPE they are or are not wearing.
- **Box:** the whole visible body. If part of the body is hidden, box only the visible part; do not guess where the hidden part would be.
- **Why keep it:** it gives a denominator. "3 people, 2 hardhats" is a compliance rate; "2 hardhats" on its own is not.

## General rules
1. **One person can carry up to three boxes:** `Person` + one of `Hardhat`/`NO-Hardhat` + one of `Safety Vest`/`NO-Safety Vest`.
2. **Never label both members of a pair on the same person** (e.g. `Hardhat` and `NO-Hardhat`).
3. **When in doubt, leave it out.** An unlabelled ambiguous case costs little; a wrong label teaches the model the wrong thing.
4. **People in reflections, on posters or on screens** are not labelled.

## Classes removed from the upstream dataset
`Mask`, `NO-Mask`, `Safety Cone`, `machinery` and `vehicle` were dropped at version generation. Masks are task-specific PPE (dusty work, confined spaces) rather than a site-wide rule, so a missing mask is not automatically a violation, and the equipment classes answer a different question from the one we set. Dropping them keeps the model focused on the two site-wide checks — head and visibility — that apply to every worker at all times.
