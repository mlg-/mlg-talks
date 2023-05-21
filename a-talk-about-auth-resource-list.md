# # A Talk About Auth! Resource List
# Emily Dickinson
* Emily Dickinson was [hella queer](https://electricliterature.com/wild-nights-with-emily/), so may you no longer think of her as a weird lonely spinster but more of a cultural outlaw. You're welcome.  
> I think of Dickinson as one such person, who declined to publish, declined to leave her home when it didn’t suit her, declined a life of straight norms. Funny how when you start saying no, you can start to dwell in possibility, you can throw open the windows and doors to the fairest visitors.

* My favorite Emily Dickinson poem:
```
If your Nerve, deny you –
Go above your Nerve –
He can lean against the Grave,
If he fear to swerve –

That’s a steady posture –
Never any bend
Held of those brass arms –
Best Giant made –

If your Soul seesaw –
Lift the Flesh door –
The Poltroon wants oxygen –
Nothing more –

(c. 1861)

Emily Dickinson Poems, Edited by Brenda Hillman
Shambhala Pocket Classics, Shambhala 1995
```
* You can go [visit](https://www.emilydickinsonmuseum.org/) Emily’s room in Amherst if we all ever leave our homes again!

# General Security Topics
## Doing Security in an Agile world
* I really like [Agile Application Security](https://www.oreilly.com/library/view/agile-application-security/9781491938836/) from O’Reilly as a big overview of running an effective AppSec program in an Agile organization. It’s a friendly, accessible, process-oriented book.

## Hashing vs. Encryption
* Auth0 resource on [hashing vs encryption](https://auth0.com/blog/how-secure-are-encryption-hashing-encoding-and-obfuscation/) (also includes discussion of encoding and obfuscation which is nice for completeness!)

## Rails Security
* Rails maintains great security [guides](https://guides.rubyonrails.org/security.html)! Read them!

## Authentication vs Authorization
* A nice tidy [resource](https://auth0.com/docs/authorization/concepts/authz-and-authn) with a table showing distinguishing characteristics between each

# Authentication
## General
* OWASP [cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) of Authentication best practices and security controls
* NIST [special publication](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-63-3.pdf) on Digital Identity Guidelines (800-63-3)

## Sessions 
* Really accessible [blog post](https://www.justinweiss.com/articles/how-rails-sessions-work/) about how sessions work in Rails
* How [CSRF protection works](https://medium.com/rubyinside/a-deep-dive-into-csrf-protection-in-rails-19fa0a42c0ef#:~:text=Briefly%2C%20Cross%2DSite%20Request%20Forgery,their%20authenticity%20with%20each%20submission.) in Rails  (in a very thorough way if you want to understand ALL the details)

## Multi-factor Authentication
* OWASP [cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html) for Multi-factor Authentication
* [More](https://medium.com/@n.moretto/two-factor-authentication-with-totp-ccc5f828b6df) on how TOTPs are generated

# Auth Technologies
## OAuth 2.0
* An [excellent talk](https://www.youtube.com/watch?v=996OiexHze0&feature=youtu.be) introducing OAuth in depth and explaining why it shouldn’t be used as an authentication mechanism by itself at the end
* Some [slides](https://drive.google.com/file/d/1VJNCJMvbR9BnjZ97rBYdapqxHlsYtzPz/view?usp=sharing) from an OWASP training I attended on SPA security explaining OAuth 2.0 and OIDC implementations and security controls
* Another good OAuth 2.0/OIDC [explainer](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc), with amazing illustrations.

## OpenID Connect
* [The standard](https://openid.net/specs/openid-connect-core-1_0.html) is pretty dense, but if you are feeling brave, it’s worth a look!
* That [iconic](https://www.youtube.com/watch?v=Oy5F9h5JqEU&feature=emb_logo) Google video lol

## JSON Web Tokens (“jot”s)
* An [intro](https://jwt.io/introduction/) to JWT.
* JWT security [cheat sheet](https://pragmaticwebsecurity.com/files/cheatsheets/jwt.pdf)
* Slightly rude/shrieky but correct articles about why JWTs are not alone a replacement for sessions. [Article one](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/), [article two](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/) (latter includes a flowchart that helps explain how the use cases for JWTs are narrow if used appropriately)
* [A spec](https://docs.google.com/document/d/1MKAvYFfwnYhydTyETLhej44tpq5QAOmWPHsoObWKzB0/edit#heading=h.4xx0cs2f7lai) about how we use JWTs in our contribution flow here at ActBlue (actual implementation was largely unchanged from this initial spec)



