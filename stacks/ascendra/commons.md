# ascendra-commons: Adopting a Module

How an Ascendra-stack project adopts a module from `ascendra-commons`. The library's own module catalogue, `MODULE-STANDARD.md` and each module's `README.md` are read in the library itself (`../ascendra-commons/`), never copied here.

## When it applies

`ascendra-commons` is built for NestJS with Drizzle on PostgreSQL (or Supabase), consumed by Next.js front ends through `ascendra-ui`. It applies only to a project whose Architecture §1 confirms that backend. For any other backend it doesn't apply at all: an untested translation of a module is never presented as the module.

## Adopting one module

Each module is its own decision, never a package deal.

1. **A real need.** The project's Requirements or Architecture names the capability now, not "might be useful later". Declining a module is a normal outcome.
2. **Model 1 or Model 2.** Every module's README says what its Model 1 ships and the signal for Model 2. Choose deliberately, and record why.

A module appears in Architecture §2 (Foundations and readiness) with its status, and in §3 (Modules) when the first unit uses it.

## The rule that is never optional: `.core` only

A module's public interface is its `.core` package: interfaces, tokens, `forRoot()` wiring. Product code imports `.core` and nothing past it (`.core.<capability>`, `.core.<family>.<stack>`, `.api`), however convenient. That is what lets a Model 1 implementation be replaced without touching business code. A plan's module block names `.core` imports only.

## Copying and changing

Modules are copied into the API repository with the commons tool, at a recorded commit. A change to a copy (a fix, a hardening such as row security on a commons table) is logged where the project's Architecture §2 says ("Changes to copied code"), with the commons commit, the change and the reason, so it can go back to the library later.
