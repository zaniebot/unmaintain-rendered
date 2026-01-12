```yaml
number: 2317
title: Migrate violations to named fields
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: violations-named-fields
created_at: 2023-01-29T01:27:26Z
updated_at: 2023-01-29T19:09:45Z
url: https://github.com/astral-sh/ruff/pull/2317
synced_at: 2026-01-12T04:52:00Z
```

# Migrate violations to named fields

---

_Pull request opened by @sciyoshi on 2023-01-29 01:27_

Fairly mechanical. Did a few of the simple cases manually to make sure things were working, and I think the rest will be easily achievable via a quick `fastmod` command.

ref #1871

---

_Comment by @charliermarsh on 2023-01-29 01:42_

`fastmod`?! TIL

---

_Comment by @sciyoshi on 2023-01-29 04:45_

@charliermarsh this should be good to go. Apologies for the large/difficult-to-review PR, but if you want to check my work, the 3rd and 4th commits (which are the largest) were achieved via this Python script which uses `fastmod` under the hood (followed by cargo fmt / cargo insta review):

```
import re
import subprocess

unary = '''\
AAndNotA(name)
AOrNotA(name)
AssertInExcept(name)
BadFilePermissions(mask)
BadQuotesDocstring(quote)
BadQuotesInlineString(quote)
BadQuotesMultilineString(quote)
BlankLineAfterLastSection(name)
BlankLineAfterSection(name)
BlankLineAfterSummary(num_lines)
BlankLineBeforeSection(name)
CapitalizeSectionName(name)
ConvertLoopToAll(all)
ConvertLoopToAny(any)
ConvertNamedTupleFunctionalToClass(name)
ConvertTypedDictFunctionalToClass(name)
DashedUnderlineAfterSection(name)
Debugger(using_type)
DictGetWithDefault(contents)
DocumentAllArguments(names)
DoubleNegation(expr)
DuplicateHandlerException(names)
DuplicateIsinstanceCall(name)
ErrorSuffixOnExceptionName(name)
FixtureParamWithoutValue(name)
FixturePositionalArgs(function)
ForwardAnnotationSyntaxError(body)
FunctionCallArgumentDefault(name)
FutureFeatureNotDefined(name)
HardcodedPasswordDefault(string)
HardcodedPasswordFuncArg(string)
HardcodedPasswordString(string)
HardcodedTempFile(string)
HashlibInsecureHashFunction(string)
IfExprWithFalseTrue(expr)
IfExprWithTrueFalse(expr)
ImportStarNotPermitted(name)
ImportStarUsed(name)
IncorrectFixtureNameUnderscore(function)
InvalidArgumentName(name)
InvalidClassName(name)
InvalidFunctionName(name)
IsLiteral(cmpop)
Jinja2AutoescapeFalse(value)
KeywordArgumentBeforeStarArgument(name)
MissingFixtureNameUnderscore(function)
MixedCaseVariableInClassScope(name)
MixedCaseVariableInGlobalScope(name)
NativeLiterals(literal_type)
NewLineAfterSectionName(name)
NoBlankLineAfterFunction(num_lines)
NoBlankLineBeforeFunction(num_lines)
NoBlankLinesBetweenHeaderAndContent(name)
NonEmptySection(name)
NonLowercaseVariableInFunction(name)
OSErrorAlias(name)
ParametrizeNamesWrongType(expected)
PercentFormatExtraNamedArguments(missing)
PercentFormatInvalidFormat(message)
PercentFormatMissingArgument(missing)
PercentFormatUnsupportedFormatCharacter(char)
RaisesTooBroad(exception)
RedundantOpenModes(replacement)
RequestWithNoCertValidation(string)
RequestWithoutTimeout(timeout)
ReturnBoolConditionDirectly(cond)
RewriteMockImport(reference_type)
SectionNameEndsInColon(name)
SectionNotOverIndented(name)
SectionUnderlineAfterName(name)
SectionUnderlineMatchesSectionLength(name)
SectionUnderlineNotOverIndented(name)
StringDotFormatExtraNamedArguments(missing)
StringDotFormatExtraPositionalArguments(missing)
StringDotFormatInvalidFormat(message)
StringDotFormatMissingArguments(missing)
TypeOfPrimitive(primitive)
UnittestAssertion(assertion)
UnnecessaryBuiltinImport(names)
UnnecessaryCallAroundSorted(func)
UnnecessaryFutureImport(names)
UnnecessarySubscriptReversal(func)
UnpackInsteadOfConcatenatingToCollectionLiteral(expr)
UnsafeYAMLLoad(loader)
UnusedClassMethodArgument(name)
UnusedFunctionArgument(name)
UnusedLambdaArgument(name)
UnusedMethodArgument(name)
UnusedNOQA(codes)
UnusedStaticMethodArgument(name)
UseContextlibSuppress(exception)
UselessObjectInheritance(name)
UselessYieldFixture(name)
UsePEP585Annotation(name)
UseTernaryOperator(contents)
YieldOutsideFunction(keyword)'''.splitlines()

for violation in unary:
    name, arg = violation.rstrip(')').split('(')
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\(pub (.+?)\);', f'{name}{{ pub {arg}: ${{1}} }}'])
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\({arg}\)', f'{name}{{ {arg} }}'])
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\(((?:[^)(]|\((?:[^)(]|\((?:[^)(]|\([^)(]*\))*\))*\))*)\)', f'{name}{{{arg}:${{1}}}}'])

binary = """\
CamelcaseImportedAsAcronym(name, asname)
CamelcaseImportedAsConstant(name, asname)
CamelcaseImportedAsLowercase(name, asname)
ConsiderMergingIsinstance(obj, types)
ConsiderUsingFromImport(module, name)
ConstantImportedAsNonConstant(name, asname)
DeprecatedUnittestAlias(alias, target)
IfExprWithTwistedArms(expr_body, expr_else)
ImportStarUsage(name, sources)
IncorrectFixtureParenthesesStyle(expected_parens, actual_parens)
KeyInDict(key, dict)
LowercaseImportedAsNonLowercase(name, asname)
NegateEqualOp(left, right)
NegateNotEqualOp(left, right)
ParametrizeValuesWrongType(values, row)
PercentFormatPositionalCountMismatch(wanted, got)
UnnecessaryDoubleCastOrProcess(inner, outer)
UseCapitalEnvironmentVariables(expected, original)""".splitlines()

for violation in binary:
    name, arg1, arg2 = re.findall(r"\w+", violation)
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\(pub (.+?), pub (.+?)\);', f'{name}{{ pub {arg1}: ${{1}}, pub {arg2}: ${{2}} }}'])
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\({arg1}, {arg2}\)', f'{name}{{ {arg1}, {arg2} }}'])
    subprocess.call(['fastmod', '--accept-all', '-m', '-e', 'rs', '-d', 'src', rf'{name}\(((?:[^)(,]|\((?:[^)(]|\((?:[^)(]|\([^)(]*\))*\))*\))*),((?:[^)(,]|\((?:[^)(]|\((?:[^)(]|\([^)(]*\))*\))*\))*)\)', f'{name}{{{arg1}:${{1}}, {arg2}:${{2}}}}'])
```

---

_Marked ready for review by @sciyoshi on 2023-01-29 04:45_

---

_Merged by @charliermarsh on 2023-01-29 18:29_

---

_Closed by @charliermarsh on 2023-01-29 18:29_

---

_Comment by @charliermarsh on 2023-01-29 18:30_

This is awesome! Thank you. I checked out locally and did a quick skim of the changed snapshots.

---

_Branch deleted on 2023-01-29 19:09_

---
