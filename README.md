<h1 align="center">👥led-bars-api-docs</h1>

## ✍️ Overview
This is the **official** FixtureAPI documentation for the [Premium Platforming](https://www.premiumplatforming.com/) LED Bar fixtures. For specific sections, refer to the table of contents below.

Looking for the Premium Platforming Discord? [Click here!](https://discord.com/invite/premium-platforming-841760990637850675)

### Table of Contents
- FAQ
	- [*How do I enable the FixtureAPI?*](#how-do-i-enable-the-fixtureapi)
	- [*How do I create a new button?*](#how-do-i-create-a-new-button)
	- [*How do I know which callback is which?*](#how-do-i-know-which-callback-is-which)
- [*FixtureAPI*](#fixtureapi)
	- [*Creating a callback*](#creating-a-callback)
	- [*Demo*](#demo)
- [*API Usage*](#api-usage)
	- [*Dimmer*](#dimmer)
	- [*Effects*](#effects)
		- [*Reference Names*](#reference-names)
		- [*Effect Names*](#effect-names)
	- [*Color*](#color)
	- [*Position*](#position)
	- [*BPM*](#bpm)
	- [*Phase*](#phase)
	- [*Fade*](#fade)
	- [*Spot*](#spot)
	- [*Reset*](#reset)
- [*Bugs or additions*](#bugs-or-additions)
- [*Credits*](#credits)
- [*License*](#license) 
<br><sub>some links may be broken, we're sorry!</sub>

## 💭 FAQ

### How do I enable the FixtureAPI?
In order to enable the API, path to `product > configuration > configuation.lua` and look for the `usingApi` variable. From there, change the value to `true`.

View the [path example](./content/path-to-configuration.gif) and the [change example](./content/change-api-status.gif) if you are having trouble finding your way.

### How do I create a new button?
In the `configuration.lua` file, scroll until you find `customButtons`. There will be three examples provided for you, aswell as an example containing all the editable properties we have provided you. Keep in mind, all of the stylistic properties such as `textColor`, `strokeColor`, etc are optional, and the properties such as `name`, `onClick`, and `link` have required fields.

View the [creation example](./content/create-new-button.gif) if you are having trouble creating a new textbutton.

```lua
{
	name = "name",
	link = "cueLink",
	onClick = {"MouseButton1Click"},
	textColor = Color3.new(1, 1, 1),
	strokeColor = Color3.new(1, 1, 1),
	textTransparency = 0,
	strokeTransparency = 0,
},
```
<sub>This is an example of a custom button.</sub>

### How do I know which callback is which?
In this product, callbacks are **index** based, meaning whatever mouse callback is provided will create an invisible bond with the callback that is on the server.

Example:
```lua
-- stripped down button
onClick = {
	[1] = "MouseButton1Down",
	[2] = "MouseButton1Up"
}
-- server callback
{
	function()
		print("callback for MouseButton1Down")
	end,
	function()
		print("callback for MouseButton1Up")
	end
}
```
<sub>Comparison of onClick{} vs .connectCallback() registration.</sub>


If there is no matching callback *(e.g. out of bounds)*, then an error will be thrown. This error is non-instrusive and will only be thrown if the missing callback is ran.


## FixtureAPI
A FixtureAPI allows representation of direct interfacing with the product itself through a script provided to the user.

### Creating a callback
identifier: `string` | `number` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">free</span></sub><br>
callbacks: `table` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>

```lua
ApiObject.connectCallback(identifier, callbacks { ... })
```
<sub>Barebones example of linking a button to callbacks.</sub>

### Demo
```lua
local initialTime = os.time()

local FixtureAPI = require(...)
repeat task.wait() until FixtureAPI.enabled

local initializationTime = os.time() - initialTime
print(`Initialized! Took {initializationTime}s.`)

FixtureAPI.connectCallback("Cue", {
	function()
		FixtureAPI.setEvent("dimmer", { func = "Power", mouseButton = "MouseButton1Click", lightOn = true })
	end
})
```

# API Usage
<details>

## Dimmer
<details>  
<summary>Dimmer Button Table</summary>
    
| func          | mouseButton      | lightOn  |
| ------------- | ---------------- | -------- |
| **Power**     | MouseButton1Down | true     |
|               | MouseButton2Down | false    |
|               | MouseButton1Up   | false    |
|               |                  |          |
| **Fade In**   | MouseButton1Down | true     |
|               |                  |          |
| **Fade Out**  | MouseButton1Down | false    |
|               |                  |          |
| **Pulse A/B** | MouseButton1Down | false    |
|               | MouseButton2Down | false    |
|               |                  |          |
| **Fade A/B**  | MouseButton1Down | true     |
|               | MouseButton2Down | true     |
|               | MouseButton1Up   | false    |
|               | MouseButton2Up   | false    |
|               |                  |          |
| **Hold A/B**  | MouseButton1Down | true     |
|               | MouseButton2Down | true     |
|               | MouseButton1Up   | false    |
|               | MouseButton2Up   | false    |
|               |                  |          |
| **Hold L/R**  | MouseButton2Down | true     |
|               | MouseButton1Down | true     |
|               | MouseButton1Up   | false    |
|               | MouseButton2Up   | false    |

Please refer to the example below for further clarification. <br> 
---
Refer to the chart to implement different functionality. In order to see what mouse button click does what, interface with the panel directly before choosing your `mouseButton` field.
```lua
ApiObject.setEvent("dimmer", { func = "Power", mouseButton = "MouseButton1Down", lightOn = true })
```

</details>

## Effects
Due to the nature of how the carousel was programmed, interfacing with effects is not as simple as one may imagine, and you will need to refer to a table in order to correctly write the name down.

In the effects pool, there is an incremental value in the top right of every effect button. This number does not represent the number of the effect, but rather the number *minus* three, so you will have to account that into any effect you are trying to toggle.

Additionally, there is an alphabetical value at the beginning of every effect, starting at a & ending at z.

### Reference Names
```lua
"a_Random Strobe", "b_Random Fade", "c_Strobe", "d_Effect_1" "e_Effect_2", "f_Effect_3", "g_Effect_4", "h_Effect_5", "i_Effect_6", "j_Effect_7", "k_Effect_8", "l_Effect_9", "m_Effect_10", "n_Effect_11", "o_Effect_12", "p_Effect_13", "q_Effect_14", "r_Effect_15", "s_Effect_16", "t_Effect_17", "u_Effect_18", "v_Effect_19", "w_Effect_20",
```
<sub>Pathing directly to the button within the panel is an option, but this option makes it easier. This only applies to "buttonReference."</sub>

### Effect Names
```lua
"Random Strobe", "Random Fade", "Strobe" "Effect_1", "Effect_2", "Effect_3", "Effect_4", "Effect_5", "Effect_6", "Effect_7", "Effect_8", "Effect_9", "Effect_10", "Effect_11", "Effect_12", "Effect_13", "Effect_14", "Effect_15", "Effect_16", "Effect_17", "Effect_18", "Effect_19", "Effect_20", "Effect_21", "Effect_22", "Effect_23" 
```
<sub>All "Effect_x" names carry the same naming mechanism. This only applies to "effectName."</sub>

**Example Below**:
```lua
ApiObject.setEvent("effect", {
	effects = {
		{
			effectName = "Random Strobe",
			buttonReference = "a_Random Strobe",
			on = true
		},
		{
			effectName = "Effect_1",
			buttonReference = "d_Effect_1",
			on = true
		},
	}
})
```

## Color
**IMPORTANT** Syntax is very different and very meticulous due to the nature of how __bad__ things can go if they are not replicated correctly. In order to do even/odd patterns you will need to send separate commands.

colorMode: `string` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>
color: `Color3` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>

Available modes to set for `colorMode` are `all`, `even`, and `odd`.

```lua
ApiObject.setEvent("color", { colorMode = "all", Color3.new(1, 1, 1)})
```

To create an even/odd color, follow this mechanism:
```lua
ApiObject.setEvent("color", { colorMode = "even", Color3.new(1, 0, 1)})
ApiObject.setEvent("color", { colorMode = "odd", Color3.new(0, 1, 0)})
```

## Position
Allows for the manipulation of various bpm  values. Available bpm values that can be controlled are `dimmer`, `movement`, & `color`.

positionIndex `number` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>


```lua
ApiObject.setEvent("position", positionIndex)
```

## ColorFX
Allows for the manipulation of several effects within the `color` category.

color `Color3` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>


```lua
ApiObject.setEvent("colorfx", color)
```

## BPM
Allows for the manipulation of various bpm  values. Available bpm values that can be controlled are `dimmer`, `movement`, & `color`.

valueName: `string` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>
value: `number` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub>

```lua
ApiObject.setEvent("bpm", { valueName = name, value = value })
```

## Phase
Allows for the manipulation of various phase values. Available phase values that can be controlled are `dimmer`, `movement`, & `color`.

phaseName: `string` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub><br>
value: `number` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub>

```lua
ApiObject.setEvent("phase", { phaseName = name, value = value })
```

## Fade
This function allows you to modify the fade time of the fixture, controlling the speed at which transitions between different states occur.

value: `number` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub>

```lua
ApiObject.setEvent("fade", { value = value })
```

## Spot
This feature provides the ability to enable or disable the spotlight functionality within the fixture.

spotValue: `boolean` <sub><span style="color:orange">required</span></sub> <sub><span style="color:green">strict</span></sub>

```lua
ApiObject.setEvent("spot", spotValue)
```

## Reset
This resets the entire fixture, restoring it to its default state.

```lua
ApiObject.setEvent("reset")
```
</details>

## 
# 👾Bugs or Additions
 
 If there are any pressing matters / additions, please report them to the [issues page](https://github.com/dr4wn/fixture-api-docs/issues). Please be realistic with yourself regarding priority *(is this really needed?, will it take long?, can i do this through another method in the product?, etc.)* & we'll make sure to get back to you as soon as possible. 
 
 
# ✍️Credits
 
 [drawn](https://github.com/dr4wn) - Programming, interface, document writing, product development, etc.<br>
 [FinnyFrang](https://roblox.com/profile/1) - Product models & visualiaztion
 
# 📜License

This license only applies to the documenation. The "LED Bars" product by Premium Platforming is proprietary software and is to be treated as such.

---
<details><summary><i>Attribution-NonCommercial-NoDerivatives 4.0 International</i></summary>
Creative Commons Corporation ("Creative Commons") is not a law firm and
does not provide legal services or legal advice. Distribution of
Creative Commons public licenses does not create a lawyer-client or
other relationship. Creative Commons makes its licenses and related
information available on an "as-is" basis. Creative Commons gives no
warranties regarding its licenses, any material licensed under their
terms and conditions, or any related information. Creative Commons
disclaims all liability for damages resulting from their use to the
fullest extent possible.

Using Creative Commons Public Licenses

Creative Commons public licenses provide a standard set of terms and
conditions that creators and other rights holders may use to share
original works of authorship and other material subject to copyright
and certain other rights specified in the public license below. The
following considerations are for informational purposes only, are not
exhaustive, and do not form part of our licenses.

     Considerations for licensors: Our public licenses are
     intended for use by those authorized to give the public
     permission to use material in ways otherwise restricted by
     copyright and certain other rights. Our licenses are
     irrevocable. Licensors should read and understand the terms
     and conditions of the license they choose before applying it.
     Licensors should also secure all rights necessary before
     applying our licenses so that the public can reuse the
     material as expected. Licensors should clearly mark any
     material not subject to the license. This includes other CC-
     licensed material, or material used under an exception or
     limitation to copyright. More considerations for licensors:
    wiki.creativecommons.org/Considerations_for_licensors

     Considerations for the public: By using one of our public
     licenses, a licensor grants the public permission to use the
     licensed material under specified terms and conditions. If
     the licensor's permission is not necessary for any reason--for
     example, because of any applicable exception or limitation to
     copyright--then that use is not regulated by the license. Our
     licenses grant only permissions under copyright and certain
     other rights that a licensor has authority to grant. Use of
     the licensed material may still be restricted for other
     reasons, including because others have copyright or other
     rights in the material. A licensor may make special requests,
     such as asking that all changes be marked or described.
     Although not required by our licenses, you are encouraged to
     respect those requests where reasonable. More considerations
     for the public:
    wiki.creativecommons.org/Considerations_for_licensees

=======================================================================

Creative Commons Attribution-NonCommercial-NoDerivatives 4.0
International Public License

By exercising the Licensed Rights (defined below), You accept and agree
to be bound by the terms and conditions of this Creative Commons
Attribution-NonCommercial-NoDerivatives 4.0 International Public
License ("Public License"). To the extent this Public License may be
interpreted as a contract, You are granted the Licensed Rights in
consideration of Your acceptance of these terms and conditions, and the
Licensor grants You such rights in consideration of benefits the
Licensor receives from making the Licensed Material available under
these terms and conditions.


Section 1 -- Definitions.

  a. Adapted Material means material subject to Copyright and Similar
     Rights that is derived from or based upon the Licensed Material
     and in which the Licensed Material is translated, altered,
     arranged, transformed, or otherwise modified in a manner requiring
     permission under the Copyright and Similar Rights held by the
     Licensor. For purposes of this Public License, where the Licensed
     Material is a musical work, performance, or sound recording,
     Adapted Material is always produced where the Licensed Material is
     synched in timed relation with a moving image.

  b. Copyright and Similar Rights means copyright and/or similar rights
     closely related to copyright including, without limitation,
     performance, broadcast, sound recording, and Sui Generis Database
     Rights, without regard to how the rights are labeled or
     categorized. For purposes of this Public License, the rights
     specified in Section 2(b)(1)-(2) are not Copyright and Similar
     Rights.

  c. Effective Technological Measures means those measures that, in the
     absence of proper authority, may not be circumvented under laws
     fulfilling obligations under Article 11 of the WIPO Copyright
     Treaty adopted on December 20, 1996, and/or similar international
     agreements.

  d. Exceptions and Limitations means fair use, fair dealing, and/or
     any other exception or limitation to Copyright and Similar Rights
     that applies to Your use of the Licensed Material.

  e. Licensed Material means the artistic or literary work, database,
     or other material to which the Licensor applied this Public
     License.

  f. Licensed Rights means the rights granted to You subject to the
     terms and conditions of this Public License, which are limited to
     all Copyright and Similar Rights that apply to Your use of the
     Licensed Material and that the Licensor has authority to license.

  g. Licensor means the individual(s) or entity(ies) granting rights
     under this Public License.

  h. NonCommercial means not primarily intended for or directed towards
     commercial advantage or monetary compensation. For purposes of
     this Public License, the exchange of the Licensed Material for
     other material subject to Copyright and Similar Rights by digital
     file-sharing or similar means is NonCommercial provided there is
     no payment of monetary compensation in connection with the
     exchange.

  i. Share means to provide material to the public by any means or
     process that requires permission under the Licensed Rights, such
     as reproduction, public display, public performance, distribution,
     dissemination, communication, or importation, and to make material
     available to the public including in ways that members of the
     public may access the material from a place and at a time
     individually chosen by them.

  j. Sui Generis Database Rights means rights other than copyright
     resulting from Directive 96/9/EC of the European Parliament and of
     the Council of 11 March 1996 on the legal protection of databases,
     as amended and/or succeeded, as well as other essentially
     equivalent rights anywhere in the world.

  k. You means the individual or entity exercising the Licensed Rights
     under this Public License. Your has a corresponding meaning.


Section 2 -- Scope.

  a. License grant.

       1. Subject to the terms and conditions of this Public License,
          the Licensor hereby grants You a worldwide, royalty-free,
          non-sublicensable, non-exclusive, irrevocable license to
          exercise the Licensed Rights in the Licensed Material to:

            a. reproduce and Share the Licensed Material, in whole or
               in part, for NonCommercial purposes only; and

            b. produce and reproduce, but not Share, Adapted Material
               for NonCommercial purposes only.

       2. Exceptions and Limitations. For the avoidance of doubt, where
          Exceptions and Limitations apply to Your use, this Public
          License does not apply, and You do not need to comply with
          its terms and conditions.

       3. Term. The term of this Public License is specified in Section
          6(a).

       4. Media and formats; technical modifications allowed. The
          Licensor authorizes You to exercise the Licensed Rights in
          all media and formats whether now known or hereafter created,
          and to make technical modifications necessary to do so. The
          Licensor waives and/or agrees not to assert any right or
          authority to forbid You from making technical modifications
          necessary to exercise the Licensed Rights, including
          technical modifications necessary to circumvent Effective
          Technological Measures. For purposes of this Public License,
          simply making modifications authorized by this Section 2(a)
          (4) never produces Adapted Material.

       5. Downstream recipients.

            a. Offer from the Licensor -- Licensed Material. Every
               recipient of the Licensed Material automatically
               receives an offer from the Licensor to exercise the
               Licensed Rights under the terms and conditions of this
               Public License.

            b. No downstream restrictions. You may not offer or impose
               any additional or different terms or conditions on, or
               apply any Effective Technological Measures to, the
               Licensed Material if doing so restricts exercise of the
               Licensed Rights by any recipient of the Licensed
               Material.

       6. No endorsement. Nothing in this Public License constitutes or
          may be construed as permission to assert or imply that You
          are, or that Your use of the Licensed Material is, connected
          with, or sponsored, endorsed, or granted official status by,
          the Licensor or others designated to receive attribution as
          provided in Section 3(a)(1)(A)(i).

  b. Other rights.

       1. Moral rights, such as the right of integrity, are not
          licensed under this Public License, nor are publicity,
          privacy, and/or other similar personality rights; however, to
          the extent possible, the Licensor waives and/or agrees not to
          assert any such rights held by the Licensor to the limited
          extent necessary to allow You to exercise the Licensed
          Rights, but not otherwise.

       2. Patent and trademark rights are not licensed under this
          Public License.

       3. To the extent possible, the Licensor waives any right to
          collect royalties from You for the exercise of the Licensed
          Rights, whether directly or through a collecting society
          under any voluntary or waivable statutory or compulsory
          licensing scheme. In all other cases the Licensor expressly
          reserves any right to collect such royalties, including when
          the Licensed Material is used other than for NonCommercial
          purposes.


Section 3 -- License Conditions.

Your exercise of the Licensed Rights is expressly made subject to the
following conditions.

  a. Attribution.

       1. If You Share the Licensed Material, You must:

            a. retain the following if it is supplied by the Licensor
               with the Licensed Material:

                 i. identification of the creator(s) of the Licensed
                    Material and any others designated to receive
                    attribution, in any reasonable manner requested by
                    the Licensor (including by pseudonym if
                    designated);

                ii. a copyright notice;

               iii. a notice that refers to this Public License;

                iv. a notice that refers to the disclaimer of
                    warranties;

                 v. a URI or hyperlink to the Licensed Material to the
                    extent reasonably practicable;

            b. indicate if You modified the Licensed Material and
               retain an indication of any previous modifications; and

            c. indicate the Licensed Material is licensed under this
               Public License, and include the text of, or the URI or
               hyperlink to, this Public License.

          For the avoidance of doubt, You do not have permission under
          this Public License to Share Adapted Material.

       2. You may satisfy the conditions in Section 3(a)(1) in any
          reasonable manner based on the medium, means, and context in
          which You Share the Licensed Material. For example, it may be
          reasonable to satisfy the conditions by providing a URI or
          hyperlink to a resource that includes the required
          information.

       3. If requested by the Licensor, You must remove any of the
          information required by Section 3(a)(1)(A) to the extent
          reasonably practicable.


Section 4 -- Sui Generis Database Rights.

Where the Licensed Rights include Sui Generis Database Rights that
apply to Your use of the Licensed Material:

  a. for the avoidance of doubt, Section 2(a)(1) grants You the right
     to extract, reuse, reproduce, and Share all or a substantial
     portion of the contents of the database for NonCommercial purposes
     only and provided You do not Share Adapted Material;

  b. if You include all or a substantial portion of the database
     contents in a database in which You have Sui Generis Database
     Rights, then the database in which You have Sui Generis Database
     Rights (but not its individual contents) is Adapted Material; and

  c. You must comply with the conditions in Section 3(a) if You Share
     all or a substantial portion of the contents of the database.

For the avoidance of doubt, this Section 4 supplements and does not
replace Your obligations under this Public License where the Licensed
Rights include other Copyright and Similar Rights.


Section 5 -- Disclaimer of Warranties and Limitation of Liability.

  a. UNLESS OTHERWISE SEPARATELY UNDERTAKEN BY THE LICENSOR, TO THE
     EXTENT POSSIBLE, THE LICENSOR OFFERS THE LICENSED MATERIAL AS-IS
     AND AS-AVAILABLE, AND MAKES NO REPRESENTATIONS OR WARRANTIES OF
     ANY KIND CONCERNING THE LICENSED MATERIAL, WHETHER EXPRESS,
     IMPLIED, STATUTORY, OR OTHER. THIS INCLUDES, WITHOUT LIMITATION,
     WARRANTIES OF TITLE, MERCHANTABILITY, FITNESS FOR A PARTICULAR
     PURPOSE, NON-INFRINGEMENT, ABSENCE OF LATENT OR OTHER DEFECTS,
     ACCURACY, OR THE PRESENCE OR ABSENCE OF ERRORS, WHETHER OR NOT
     KNOWN OR DISCOVERABLE. WHERE DISCLAIMERS OF WARRANTIES ARE NOT
     ALLOWED IN FULL OR IN PART, THIS DISCLAIMER MAY NOT APPLY TO YOU.

  b. TO THE EXTENT POSSIBLE, IN NO EVENT WILL THE LICENSOR BE LIABLE
     TO YOU ON ANY LEGAL THEORY (INCLUDING, WITHOUT LIMITATION,
     NEGLIGENCE) OR OTHERWISE FOR ANY DIRECT, SPECIAL, INDIRECT,
     INCIDENTAL, CONSEQUENTIAL, PUNITIVE, EXEMPLARY, OR OTHER LOSSES,
     COSTS, EXPENSES, OR DAMAGES ARISING OUT OF THIS PUBLIC LICENSE OR
     USE OF THE LICENSED MATERIAL, EVEN IF THE LICENSOR HAS BEEN
     ADVISED OF THE POSSIBILITY OF SUCH LOSSES, COSTS, EXPENSES, OR
     DAMAGES. WHERE A LIMITATION OF LIABILITY IS NOT ALLOWED IN FULL OR
     IN PART, THIS LIMITATION MAY NOT APPLY TO YOU.

  c. The disclaimer of warranties and limitation of liability provided
     above shall be interpreted in a manner that, to the extent
     possible, most closely approximates an absolute disclaimer and
     waiver of all liability.


Section 6 -- Term and Termination.

  a. This Public License applies for the term of the Copyright and
     Similar Rights licensed here. However, if You fail to comply with
     this Public License, then Your rights under this Public License
     terminate automatically.

  b. Where Your right to use the Licensed Material has terminated under
     Section 6(a), it reinstates:

       1. automatically as of the date the violation is cured, provided
          it is cured within 30 days of Your discovery of the
          violation; or

       2. upon express reinstatement by the Licensor.

     For the avoidance of doubt, this Section 6(b) does not affect any
     right the Licensor may have to seek remedies for Your violations
     of this Public License.

  c. For the avoidance of doubt, the Licensor may also offer the
     Licensed Material under separate terms or conditions or stop
     distributing the Licensed Material at any time; however, doing so
     will not terminate this Public License.

  d. Sections 1, 5, 6, 7, and 8 survive termination of this Public
     License.


Section 7 -- Other Terms and Conditions.

  a. The Licensor shall not be bound by any additional or different
     terms or conditions communicated by You unless expressly agreed.

  b. Any arrangements, understandings, or agreements regarding the
     Licensed Material not stated herein are separate from and
     independent of the terms and conditions of this Public License.


Section 8 -- Interpretation.

  a. For the avoidance of doubt, this Public License does not, and
     shall not be interpreted to, reduce, limit, restrict, or impose
     conditions on any use of the Licensed Material that could lawfully
     be made without permission under this Public License.

  b. To the extent possible, if any provision of this Public License is
     deemed unenforceable, it shall be automatically reformed to the
     minimum extent necessary to make it enforceable. If the provision
     cannot be reformed, it shall be severed from this Public License
     without affecting the enforceability of the remaining terms and
     conditions.

  c. No term or condition of this Public License will be waived and no
     failure to comply consented to unless expressly agreed to by the
     Licensor.

  d. Nothing in this Public License constitutes or may be interpreted
     as a limitation upon, or waiver of, any privileges and immunities
     that apply to the Licensor or You, including from the legal
     processes of any jurisdiction or authority.

=======================================================================

Creative Commons is not a party to its public
licenses. Notwithstanding, Creative Commons may elect to apply one of
its public licenses to material it publishes and in those instances
will be considered the “Licensor.” The text of the Creative Commons
public licenses is dedicated to the public domain under the CC0 Public
Domain Dedication. Except for the limited purpose of indicating that
material is shared under a Creative Commons public license or as
otherwise permitted by the Creative Commons policies published at
creativecommons.org/policies, Creative Commons does not authorize the
use of the trademark "Creative Commons" or any other trademark or logo
of Creative Commons without its prior written consent including,
without limitation, in connection with any unauthorized modifications
to any of its public licenses or any other arrangements,
understandings, or agreements concerning use of licensed material. For
the avoidance of doubt, this paragraph does not form part of the
public licenses.

Creative Commons may be contacted at creativecommons.org.
</details>
