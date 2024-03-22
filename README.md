# gryphon-sandbox


To get started you need Cargo's bin directory ($HOME/.cargo/bin) in your PATH
environment variable. This has not been done automatically.

To configure your current shell, you need to source
the corresponding env file under $HOME/.cargo.

This is usually done by running one of the following (note the leading DOT):
. "$HOME/.cargo/env"            # For sh/bash/zsh/ash/dash/pdksh
source "$HOME/.cargo/env.fish"  # For fish

----------------------
'''code

use bonsai_bt::{Behavior, Sequence, Invert, Condition};

fn main() {
    // Create a sequence of actions: Move to waypoint, wait, and repeat
    let patrol_tree = Sequence::new(vec![
        MoveToWaypoint::new(),
        Wait::new(2.0), // Wait for 2 seconds
    ]);

    // Invert the result: If patrol_tree fails, it means the enemy is not patrolling
    let inverted_patrol = Invert::new(patrol_tree);

    // Check if the player is nearby
    let player_nearby = Condition::new(|| is_player_nearby());

    // If player is nearby, attack; otherwise, patrol
    let behavior_tree = Sequence::new(vec![
        player_nearby,
        Attack::new(),
        inverted_patrol,
    ]);

    // Execute the behavior tree each tick
    loop {
        behavior_tree.tick();
    }
}
'''