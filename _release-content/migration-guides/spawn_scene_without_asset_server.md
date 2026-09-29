---
title: `World::spawn_scene` no longer requires `AssetServer`
pull_requests: [25917]
---

`World::spawn_scene`, `World::spawn_scene_list`, and `EntityWorldMut::apply_scene` no longer require an `AssetServer` or `Assets<ScenePatch>` resource to be present in `World` when the scene being spawned declares no external asset dependencies.

`ScenePatch::load` and `SceneListPatch::load` now accept `Option<&AssetServer>` and return `Result<Self, ResolveSceneError>`. If asset dependencies are declared by the scene but `AssetServer` is absent, they return `Err(ResolveSceneError::MissingAssetServer)`.

Existing code that calls `World::spawn_scene` with an initialized `AssetServer` is unaffected. Custom callers of `ScenePatch::load` or `SceneListPatch::load` must pass `Option<&AssetServer>` and handle the returned `Result`.
