### Index

+ src
  + main
    + scala
      + [Bundle.scala](../../main/scala/Bundle.md)
      + [DepsTower.scala](../../main/scala/DepsTower.md)
      + [Distribution.scala](../../main/scala/Distribution.md)
      + [InstallMethods.scala](../../main/scala/InstallMethods.md)
      + [package.scala](../../main/scala/package.md)
      + [ZipUnionHLists.scala](../../main/scala/ZipUnionHLists.md)
  + test
    + scala
      + [BundleTest.scala](BundleTest.md)
      + [InstallWithDepsSuite.scala](InstallWithDepsSuite.md)
      + [InstallWithDepsSuite_Aux.scala](InstallWithDepsSuite_Aux.md)

------


```scala
package ohnosequences.statika.tests

import shapeless._
import shapeless.poly._
import ohnosequences.statika._
import ohnosequences.typesets._

object genericPrintln extends (TypeSet -> Unit)(println(_))

class NameSuite extends org.scalatest.FunSuite { 
  object Foo {
    case object bun extends Bundle()
  }
  println(Foo.bun.fullName)
  println(Foo.bun.name)
}

class BuhSuite extends org.scalatest.FunSuite {

  case class bubun(s: String) extends Bundle()

  case object buh extends Bundle()
  case object yeah extends Bundle()
  case object hey extends Bundle(buh :~: yeah :~: ∅)
  case object limit extends Bundle(hey :~: ∅)

  // was failing here before: (because couldn't flatten deps)
  case object l extends Bundle(limit :~: ∅)
  case object r extends Bundle(limit :~: ∅)
  case object q extends Bundle(l :~: r :~: limit :~: ∅)
  case object w extends Bundle(r :~: hey :~: ∅)
  case object e extends Bundle(q :~: w :~: ∅)

  test("output tower") {
    e.depsTower.map(genericPrintln); println()
  }
}

```

