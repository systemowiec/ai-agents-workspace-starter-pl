# Konwencje językowe - starter kit dla agentów

## Zasada ogólna

- **Opisy, instrukcje i dokumentacja**: po polsku, z pełną polską diakrytyką (ą, ć, ę, ł, ń, ó, ś, ź, ż). Poprawność gramatyczna i naturalność są kluczowe.
- **Słowa kluczowe dla agentów**: celowo po angielsku tam, gdzie model ma kojarzyć termin 1:1 z działaniem (np. review, flow, DT). Nie traktuj tego jako błąd tłumaczenia.

## Słowa kluczowe pozostające po angielsku (dla spójności z LLM)

Te terminy używamy w instrukcjach po angielsku, żeby agent jednoznacznie rozpoznawał polecenia i nazwy artefaktów:

| Termin | Użycie |
|--------|--------|
| review | przegląd kodu, etap workflow (review -> review-*.md) |
| flow / workflow | przebieg pracy, sekwencja kroków |
| DT | Dokument Techniczny (DT-NNN) |
| feature | funkcjonalność (w kontekście branch/scope można też: funkcjonalność) |
| audit | audyt kodu (pełny przegląd) |
| scoring | punktacja w Quality Gate |
| compaction | kompresja kontekstu (techniczny termin) |
| file-based | przechowywane w plikach |

## Pisownia

- W plikach instrukcji agenta i dokumentacji użytkowej stosujemy ZAWSZE **pełną polską diakrytykę**. Jest to świadoma decyzja architektoniczna, ponieważ agenci AI mierzalnie lepiej radzą sobie z poprawną polską morfologią (aspekt, końcówki), która bez diakrytyki bywa wieloznaczna.

## Kiedy coś poprawiać

- **Popraw**: błędy gramatyczne (zgodność rodzaju, liczby, przypadków), literówki, nieistniejące słowa, nienaturalne zdania.
- **Zostaw**: celowe angielskie słowa kluczowe z powyższej listy.
